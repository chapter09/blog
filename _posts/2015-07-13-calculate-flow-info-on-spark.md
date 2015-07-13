---
layout: post
title: "Calculate Flow info. on Spark"
description: ""
categories: 
tags: []
---
{% include JB/setup %}


##Calculate Flow _info._ on Spark 

Hao WANG, 12 July, 2015

---

Spark version 1.0.0

####1. To construct the full DAG

**File**: `/core/src/main/scala/org/apache/spark/scheduler/DAGScheduler.scala`

First, to record the DAG or Stage dependencies, claim a HashMap at the head of the file.

	+  private[scheduler] val stageDep = new HashMap[Stage, Stage]


Insert the `jobAnalysis` method in `handleJobSubmitted` method.


	/** Job Analysis, output stages */
	+  private def jobAnalysis(stage: Stage) {
	+
	+    def wrapper(curStage: Stage, childStage: Stage) {
	+      val jobId = activeJobForStage(stage)
	+      if (jobId.isDefined) {
	+        val missing = getMissingParentStages(stage).sortBy(_.id)
	+        if (!waitingStages(stage) && !runningStages(stage) && !failedStages(stage)) {
	+          if (missing != Nil) {
	+            for (parent <- missing) {
	+              stageDep(parent) = curStage
	+              wrapper(parent, curStage)
	+            }
	+          }
	+        }
	+      }
	+    }
	+
	+    wrapper(stage, stage)
	+    logInfo("--->JobDep: " + stageDep.toString)
	+  }

Everytime when DAGScheduler submits a new Stage, the `handleJobSubmitted` will be called. Then by analyzing the dependencies of the newly submitted Stage, `jobAnalysis` constructs the full DAG for current job. 


####2. Data Partition ID, size and location

**File**: `/core/src/main/scala/org/apache/spark/scheduler/ShuffleMapTask.scala`

The ShuffleMapTask are distributed to each worker to do the Shuffle write, which is similar to the _Map_ pharse in MapReduce/Hadoop. 

    override def runTask(context: TaskContext): MapStatus = {
    // Deserialize the RDD using the broadcast variable.
    val ser = SparkEnv.get.closureSerializer.newInstance()
    val (rdd, dep) = ser.deserialize[(RDD[_], ShuffleDependency[_, _, _])](
      ByteBuffer.wrap(taskBinary.value), Thread.currentThread.getContextClassLoader)

    metrics = Some(context.taskMetrics)
    var writer: ShuffleWriter[Any, Any] = null
    try {
      val manager = SparkEnv.get.shuffleManager
      writer = manager.getWriter[Any, Any](dep.shuffleHandle, partitionId, context)
      writer.write(rdd.iterator(partition, context).asInstanceOf[Iterator[_ <: Product2[Any, Any]]])
      return writer.stop(success = true).get
    } catch {
      case e: Exception =>
        if (writer != null) {
          writer.stop(success = false)
        }
        throw e
    } finally {
      context.markTaskCompleted()
    }
  }

The `manager` in above code refers to the `ShuffleBlockManager` defined in the following file.
	  
**File**: `/core/src/main/scala/org/apache/spark/storage/ShuffleBlockManager.scala`


    def forMapTask(shuffleId: Int, mapId: Int, numBuckets: Int, serializer: Serializer,
      writeMetrics: ShuffleWriteMetrics) = {
    new ShuffleWriterGroup {
      shuffleStates.putIfAbsent(shuffleId, new ShuffleState(numBuckets))
      private val shuffleState = shuffleStates(shuffleId)
      private var fileGroup: ShuffleFileGroup = null

      val writers: Array[BlockObjectWriter] = if (consolidateShuffleFiles) {
        fileGroup = getUnusedFileGroup()
        Array.tabulate[BlockObjectWriter](numBuckets) { bucketId =>
          val blockId = ShuffleBlockId(shuffleId, mapId, bucketId)
          blockManager.getDiskWriter(blockId, fileGroup(bucketId), serializer, bufferSize,
            writeMetrics)
        }
      } else {
        Array.tabulate[BlockObjectWriter](numBuckets) { bucketId =>
          val blockId = ShuffleBlockId(shuffleId, mapId, bucketId)
          val blockFile = blockManager.diskBlockManager.getFile(blockId)
          // Because of previous failures, the shuffle file may already exist on this machine.
          // If so, remove it.
          if (blockFile.exists) {
            if (blockFile.delete()) {
              logInfo(s"Removed existing shuffle file $blockFile")
            } else {
              logWarning(s"Failed to remove existing shuffle file $blockFile")
            }
          }
          blockManager.getDiskWriter(blockId, blockFile, serializer, bufferSize, writeMetrics)
        }
      }
  ...
  }

Here is the definition of data block ID: `val blockId = ShuffleBlockId(shuffleId, mapId, bucketId)`. It's made up of `shuffleId`, `mapId`, and `bucketId`. 

**IMPORTANT**: The `bucketId` can be regarded as the `reduceId`, to identify the specific reducer, who will request this data block later.

 

**File**: `/core/src/main/scala/org/apache/spark/shuffle/hash/HashShuffleWriter.scala`

The write method will be called on an executor, and the result blocks of a map task will be put into local disk. Therefore, here we can obtain **location** (_i.e._, executor location) and **size** of intermediate data for a specific Stage before a shuffle takes place.

	/** Write a bunch of records to this task's output */
	override def write(records: Iterator[_ <: Product2[K, V]]) = {
	 val iter = if (dep.aggregator.isDefined) {
	   if (dep.mapSideCombine) {
	     dep.aggregator.get.combineValuesByKey(records, context)
	   } else {
	     records
	   }
	 } else if (dep.aggregator.isEmpty && dep.mapSideCombine) {
	   throw new IllegalStateException("Aggregator is empty for map-side combine")
	 } else {
	   records
	 }
	
	 for (elem <- iter) {
	   val bucketId = dep.partitioner.getPartition(elem._1)
	   shuffle.writers(bucketId).write(elem)
	 }
	}

The `shuffle.writers(bucketId)` is a writer object initiated from the following class:

**File**: `/core/src/main/scala/org/apache/spark/storage/BlockObjectWriter.scala`

	private[spark] class DiskBlockObjectWriter(
	    blockId: BlockId,
	    file: File,
	    serializer: Serializer,
	    bufferSize: Int,
	    compressStream: OutputStream => OutputStream,
	    syncWrites: Boolean,
	    // These write metrics concurrently shared with other active BlockObjectWriter's who
	    // are themselves performing writes. All updates must be relative.
	    writeMetrics: ShuffleWriteMetrics)
	  extends BlockObjectWriter(blockId)
	  with Logging
	{
	  /** Intercepts write calls and tracks total time spent writing. Not thread safe. */
	  private class TimeTrackingOutputStream(out: OutputStream) extends OutputStream {
	    def write(i: Int): Unit = callWithTiming(out.write(i))
	    override def write(b: Array[Byte]) = callWithTiming(out.write(b))
	    override def write(b: Array[Byte], off: Int, len: Int) = callWithTiming(out.write(b, off, len))
	    override def close() = out.close()
	    override def flush() = out.flush()
	  }

A detailed blog on Spark Shuffle is [here](https://github.com/JerryLead/SparkInternals/blob/master/markdown/4-shuffleDetails.md). I also posted an unfinished blog on Shuffle. Please click [here](http://www.haow.ca/blog/2014/07/16/shuffle-in-spark/) if you are interested.

####3. Shuffle ID

**File**: `/core/src/main/scala/org/apache/spark/Dependency.scala`

In this file, a Shuffle ID is generated when a RDD calls a transformation which creates shuffle dependency between RDDs, _e.g._, `reduceByKey`.

	class ShuffleDependency[K, V, C](
	    @transient _rdd: RDD[_ <: Product2[K, V]],
	    val partitioner: Partitioner,
	    val serializer: Option[Serializer] = None,
	    val keyOrdering: Option[Ordering[K]] = None,
	    val aggregator: Option[Aggregator[K, V, C]] = None,
	    val mapSideCombine: Boolean = false)
	  extends Dependency[Product2[K, V]] {
	
	  override def rdd = _rdd.asInstanceOf[RDD[Product2[K, V]]]
	
	  val shuffleId: Int = _rdd.context.newShuffleId()
	
	  val shuffleHandle: ShuffleHandle = _rdd.context.env.shuffleManager.registerShuffle(
	    shuffleId, _rdd.partitions.size, this)
	
	  _rdd.sparkContext.cleaner.foreach(_.registerShuffleForCleanup(this))
	}

`val shuffleId: Int = _rdd.context.newShuffleId()` generates a new Shuffle ID.

Shuffle ID identify the dependency between two Stages. The child Stage will fetch data from its parent Stages according to the Shuffle ID. Thus, by looking up the Shuffle ID, we are able to locate the group of data blocks on workers.


###4. Task Assignment

After the parent Stages are finished, the child Stage is ready to launch. Then tasks in this child Stage will be assigned to each worker. A task is a capsule which can be executed independently, including all the information the worker needs to run, _e.g._, `reduceId`.

**File**: `/core/src/main/scala/org/apache/spark/scheduler/DAGScheduler.scala'

    val tasks: Seq[Task[_]] = if (stage.isShuffleMap) {
      partitionsToCompute.map { id =>
        val locs = getPreferredLocs(stage.rdd, id)
        val part = stage.rdd.partitions(id)
        new ShuffleMapTask(stage.id, taskBinary, part, locs)
      }
    } else {
      val job = stage.resultOfJob.get
      partitionsToCompute.map { id =>
        val p: Int = job.partitions(id)
        val part = stage.rdd.partitions(p)
        val locs = getPreferredLocs(stage.rdd, p)
        new ResultTask(stage.id, taskBinary, part, locs, id)
      }
    }

For the Shuffle read phase or reduce phase, tasks are `ResultTask`, _i.e._, `ResultTask(stage.id, taskBinary, part, locs, id)`. Thus, `part.index` is the `reduceId`. How to find the connection between `part.index` and `reduceId` is a long deduction chain.

Here is a brief description:

DAGScheduluer (_generate ResultTasks, assign **partition**_) --> executor (_run ResultTasks_) --> ResultTask (_run func_) --> ShuffledRDD.compute (_start fetching data_) --> BlockStoreShuffleFetcher.fetch (_fetch data according to **reduceId**_)


###5. Calculation

* Section 1: full DAG, the Stage ID and dependencies between Stages. 
* Section 2: the location, size of data partitions from finished Stages.
* Section 3: Shuffle ID, identifies the data corresponding to the upcoming Stage.
* Section 4: Task assignment, _i.e._, task-to-worker mapping.

Section 2 refers to the size, location of the data, which is the **source** and **size** of network flows. Section 4 tells that which data partition a worker will request, which is the **destination** of the flows.
