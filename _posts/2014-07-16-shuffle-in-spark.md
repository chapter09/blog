---
layout: post
title: "Shuffle in Spark"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

###1. Overview

	MappedRDD -- ShuffleDependency -- ShuffleMapRDD
		|									|
	shuffleMapTask						rdd.iterator
		|									|
	shuffleWrite				rdd.compute -> shuffleFetch

###2. Shuffle Write
	
Actually, a stage will be marked as `isShuffleMap`, then it creates ShuffleMapTasks in the `submitMissingTasks`.

	
      // Write the map output to its associated buckets.
      for (elem <- rdd.iterator(split, context)) {
        val pair = elem.asInstanceOf[Product2[Any, Any]]
        val bucketId = dep.partitioner.getPartition(pair._1)
        shuffle.writers(bucketId).write(pair)
      }

In the code above, the `shuffleMapTask` will compute and get a result `pair`, which is a Key-Value pair. Then, `HashPartitioner` (default) with the Key as input generates a `bucketId`. In a WikipediaPageRank example, the `pair` and `bucketId` is like below:
	
	(Nové Hrady (Ústí nad Orlicí District),PRVertex(value=0.000001, outEdges.length=13, active=true)) 26
	(Sobno,PRVertex(value=0.000001, outEdges.length=19, active=true)) 23

`shuffle.writes` is actually a `Array[BlockObjectWriter]`:

	val writers: Array[BlockObjectWriter] = if (consolidateShuffleFiles) {
	        fileGroup = getUnusedFileGroup()
	        Array.tabulate[BlockObjectWriter](numBuckets) { bucketId =>
	          val blockId = ShuffleBlockId(shuffleId, mapId, bucketId)
	          blockManager.getDiskWriter(blockId, fileGroup(bucketId), serializer, bufferSize)
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
	          blockManager.getDiskWriter(blockId, blockFile, serializer, bufferSize)
	        }
	      }



####a. Parameter _consolidateShuffleFiles_

`shuffle.writers` returns a series of `DiskBlockObjectWriter`, which means the Spark intermediate data are stored in the disk. The writers keep an *one-to-one* relationship with `bucketId`. The corresponding relationship between `bucketId`, 'blockId/ShuffleBlockId' and 'shuffleFile' could be depicted like below:

* `consolidateShuffleFiles` == False
	
		ShuffleMapTask_1 --> 
		 mapId_1 -->  |- bucketId_1 --> blockId_1 --> shuffleFile_1 
		              |- bucketId_2 --> blockId_2 --> shuffleFile_2 
					   |- bucketId_3 --> blockId_3 --> shuffleFile_3 
         	  		   |- bucketId_4 --> blockId_4 --> shuffleFile_4 
					   |...
					   |_ (numPartition / reduceNumber)

		ShuffleMapTask_2 --> 
		  mapId_2 --> |- bucketId_1 --> blockId_5 --> shuffleFile_1 
			 	  	  |- bucketId_2 --> blockId_6 --> shuffleFile_2 
					  |- bucketId_3 --> blockId_7 --> shuffleFile_3 
					  |- bucketId_4 --> blockId_8 --> shuffleFile_4 
					  |...
					  |_ (numPartition / reduceNumber)

`blockId` is combined by `shuffleId`, `mapId` and `reduceId`. To simplify, just tag the `blockId` as `blockId_#`
		
Then for a single Worker, the number of intermediate files is __ShuffleMapTask Number__ x __ReduceTask Number__, in short as __M__ x __R__.


* `consolidateShuffleFiles` == True

		mapId_1 -->   
		  |- bucketId_1 --> blockId_1 --> --|
		  |- bucketId_2 --> blockId_2 -->   |
		  |- bucketId_3 --> blockId_3 -->   |
		  |- bucketId_4 --> blockId_4 -->   |
		  |...	                            |--> shuffleFile_1
		  |_ (numPartition / reduceNumber)  |                         
		  		                    		 |	       				                            |             
		mapId_2 -->                         |  
		  |- bucketId_1 --> blockId_5 --> --| 
		  |- bucketId_2 --> blockId_6 -->  
	  	  |- bucketId_3 --> blockId_7 -->  
  		  |- bucketId_4 --> blockId_8 -->  
		  |...
		  |_ (numPartition / reduceNumber)
								  	

####b. Map and Reduce




###3. Shuffle Fetch

