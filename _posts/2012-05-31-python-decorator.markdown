---
author: chapter09
comments: true
date: 2012-05-31 06:17:48+00:00
layout: post
slug: python-decorator
title: Python Decorator
wordpress_id: 740
categories:
- Tech
---

[http://stackoverflow.com/questions/739654/understanding-python-decorators](http://stackoverflow.com/questions/739654/understanding-python-decorators)

python decorator根据decorator和被修饰函数的关系可以分为四种：
1、decorator和func都是函数：
[code lang="python"]
def before(f):
    def wrapper():
        print 'before function'
        f()
    return wrapper
 
def after(f):
    def wrapper():
        f()
        print 'after function'
    return wrapper
 
@before
@after
def func():
    print 'this is function'
 
if __name__ == '__main__':
    func()
[/code]
2、decorator是一个类，而被修饰函数是函数：
[code lang="python"]
class myDecorator(object):

    def __init__(self, f):
        print "inside myDecorator.__init__()"
        f() # Prove that function definition has completed

    def __call__(self):
        print "inside myDecorator.__call__()"

@myDecorator
def aFunction():
    print "inside aFunction()"
[/code]
3、decorator是一个函数，被修饰函数是类中的method：
[code lang="python"]
def addDashes(fn): # notice it takes a function as an argument
    def newFunction(self): # define a new function
        print "---"
        fn(self) # call the original function
        print "---"
    return newFunction
    # Return the newly defined function - it will "replace" the original

class foo:
    @addDashes
    def bar(self):
        print "hi"

    @addDashes
    def foobar(self):
        print "hi again"
[/code]
4. decorator是类，而被修饰函数是类中的method:
[code lang="python"]

class myDecorator(object):
        def __init__(self, f):
                print "inside myDecorator.__init__()"
                f(self) # Prove that function definition has completed
        def __call__(self):
                print "inside myDecorator.__call__()"
class Test(object):
        def __init__(self):
                pass

        @myDecorator
        def aFunction(self):
                print "inside aFunction()"
                print self.__class__.__name__
[/code]

4中的实例代码执行结果：
inside myDecorator.__init__()
inside aFunction()
myDecorator

实际上在那天knowledge sharing的会议上，我说到Decorator的时候，老一辈程序员同事无一不想到的是C的预编译宏，的确decorator在python中也是提前执行的。不论@deco放在哪个位置，即使所修饰的代码没有被调用。
