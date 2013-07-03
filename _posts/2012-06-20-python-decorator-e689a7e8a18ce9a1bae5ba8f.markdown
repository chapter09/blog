---
author: chapter09
comments: true
date: 2012-06-20 18:22:05+00:00
layout: post
slug: python-decorator-%e6%89%a7%e8%a1%8c%e9%a1%ba%e5%ba%8f
title: Python Decorator 执行顺序
wordpress_id: 802
categories:
- Tech
tags:
- Python
---

Python Decorator类似于C的预编译宏，因此在代码执行函数调用前，或者执行main函数前就已经运行完成了decorator。但是执行顺序在decorator引入到类，以及与python 内建函数如__call__等结合使调用关系复杂以后，使得代码可读性就下降。为了理解好Decorator的用法，下面写了实例代码：
[code lang="python"]

class myDecorator(object):
        def __init__(self, func=None, num=None):
                print "1 inside myDecorator.__init__()"
                self.func = func # Prove that function definition has completed
                print "2 __init__ ends"

        def __call__(self, counter):
                print "3 myDecorator.__call__"
                print type(self.print_out)
                print "3 end"
                return self.print_out

        def print_out(self, counter):
                print "4 myDecorator.print_out"
                print "5 ",
                print counter
                print "type:"
                print type(counter)


#class Test(object):
#       def __init__(self):
#               pass

#       @myDecorator(num = 1)
#       def __call__(self, counter):
#               print "Test.call: ",
#               return counter

#test = Test()
#test(11)

@myDecorator(num = 1)
def abc(counter):
        print "Test.call: ",
        return counter

abc(11)
[/code]

实例中被修饰函数有两种，被注释掉的是class method的函数，另一个则是def定义的普通函数。
输出为：
1 inside myDecorator.__init__()
2 __init__ ends
3 myDecorator.__call__

3 end
4 myDecorator.print_out
5  11
type:

如果去掉abc(11)的调用，输出为：
1 inside myDecorator.__init__()
2 __init__ ends
3 myDecorator.__call__

3 end

调用abc(11)可以理解为： 
func(11) = myDeco(num = 1)(func(11))
其中myDeco(num = 1)为myDeco的instance，
而myDeco()则是调用myDeoc的__call__方法，而__call__返回的是print_out函数，而func(11)返回的就是11
因此最终函数变为func(11) = print_out(11)，即最终执行的为print_out函数。





