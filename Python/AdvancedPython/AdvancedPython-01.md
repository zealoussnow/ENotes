## Advanced Python

## 语法最佳实践——低于类级

### 一、 列表推导(List Comprehensions)

	[ i for i in range(10) if i % 2 == 0]

每当要对序列中的内容进行循环处理时，就应该尝试用List Comprehensions来代替它。

### 二、 迭代器和生成器(Iterator & Generator)

#### 1. 迭代器(Iterator)

迭代器只不过是一个实现迭代器协议的容器对象，它基于两个方法：

* next 返回容器的下一个项目

* \_\_iter\_\_ 返回迭代器本身

#### 2. 生成器(Generator)

生成器提供了一种出色的方法，使得需要返回一系列元素的函数所需的代码更加简单、高效。
基于yield指令，可以暂停一个函数并返回中间结果。该函数将保存执行环境并且可以在必要
时恢复。

如：

	def fib():
		a, b = 0, 1
		while True:
			yield b
			a, b = b, a + b
	f = fib
	[f.next() for i in range(10)]

该函数将返回一个特殊的迭代器，也就是generator对象，它知道如何保存执行环境。对
它的调用时不确定的，每次都将产生序列中的下一个元素。这种语法很简洁，算法的不确定
特性并没有影响代码的可读性。不必提供使函数可停止的方法。实际上，这看上去像是用
伪代码设计的序列一样。

---------------------------

python引入的与生成器相关的最后一个特性是提供了与next方法调用的代码进行交互的功能。
yield将变成一个表达式，而一个值可以通过一个send函数来传递。

如：

	def psychologist():
		print 'Please input OK'
    	while True:
        answer = (yield)
        if answer is not None:
            if answer.endswith(','):
                print 'The answer is not ok'
            elif 'good' in answer:
                print "That's good, go on"
            elif 'bad' in answer:
                print "Don't be so negative"
	>>> from demo import *
	Please input OK
	>>> f.send('ok')
	>>> f.send('good')
	That's good, go on
	>>> f.send('bad')
	Don't be so negative
	>>> f.send('bad,')
	The answer is not ok

send的工作机制与next一样，但是yield将变成能够返回传入的值，
因此这个函数可以根据客户端改变其行为，同时还添加了throw和
close两个函数，以完成该行为。它们向生成器抛出一个错误：

* throw允许客户端代码传入要抛出的任何类型的异常

* close的工作方式是相同的，但是将会抛出一个特定的异常--
GeneratorExit，在这种情况下，生成器必须再次抛出GeneratorExit
或StopIteration异常

* finally部分在之前的版本中是不允许使用的，它将捕获任何未被
捕获的close和throw调用，是完成清理工作的推荐方式

#### 3. 协同程序(coroutine)

协同程序是可以挂起，恢复，并且有多个进入点的函数。

生成器几乎就是协同程序，添加send，throw和close，其初始意图
就是为该语言提供一种类似协同程序的特性。

* [PEP342](http://www.python.org/dev/peps/pep-0342)实例化了
生成器的行为，也提供了创建协同程序的调度程序的完整实例，这个模式
被称为Trampoline，可以被看作是生成和消费数据的协同程序之间的媒介。
它使用一个队列将协同程序连接在一起。

* 在PyPI中的multitask模块(用easy_install multitask安装)实现了
这一模式，使用也十分简单：

如：

	import multitask
	import time

	def coroutine_1():
		for i in range(3):
			print 'c1'
			yield i

	def coroutine_2():
		for i in range(3):
			print 'c2'
			yield i

	>>> multitask.add(coroutine_1())
	>>> multitask.add(coroutine_2())
	>>> multitask.run()

另一种协同程序的实现：greenlet

#### 4. 生成器表达式(Genexp)

python为编写针对序列的简单生成器提供了一种快捷方式。可以用一种类似
列表推导的语法来代替yield。在此，使用圆括号替代中括号：

如：

	iter = (x ** 2 for x in range(10) if x % 2 == 0)
	for el in iter:
		print el

#### 5. itertools模块

##### (1). islice: 窗口迭代器

islice将返回一个运行在序列的子分组之上的迭代器

如：

	def starting_at_five():
    value = raw_input().strip()
    while value != '':
        for el in itertools.islice(value.split(), 4, None):
            yield el
        value = raw_input().strip()

	>>> iter = starting_at_five()
	>>> iter.next()
	one two three four five six
	'five'
	>>> iter.next()
	'six'

当需要抽取位于流中特定位置的数据时，都可以使用islice。

##### (2). tee: 往返式迭代器

迭代器将消费其处理的序列，但它不会往回处理。tee提供了在一个序列
之上运行多个迭代器的模式。如果提供第一次运行的信息，就能够帮助
我们再次基于这些数据运行。例如：读取文件的表头可以在运行一个
处理之前提供特性信息，

如：

	def with_head(iterable, headsize = 1):
		a, b = itertools.tee(iterable)
    	return list(itertools.islice(a, headsize)), b
	>>> seq = [1, 2, 3, 4, 5]
	>>> with_head(seq)
	([1], <itertools.tee object at 0xb82a28>)
	>>> with_head(seq, 2)
	([1, 2], <itertools.tee object at 0xb82ab8>)
	>>> with_head(seq, 3)
	([1, 2, 3], <itertools.tee object at 0xb82a28>)
	>>> with_head(seq, 4)
	([1, 2, 3, 4], <itertools.tee object at 0xb82ab8>)

##### (3). groupby: uniq迭代器

类似于unix命令uiq，它可以对来自一个迭代器的重复元素进行分组。
groupby的一个应用实例是使用RLE来压缩数据，
如：

	from itertools import groupby

	def compress(data):
    	return ((len(list(group)), name) for name, group in groupby(data))

	def decompress(data):
    	return (car * size for size, car in data)

	>>> list(compress('get uuuuuup'))
	[(1, 'g'), (1, 'e'), (1, 't'), (1, ' '), (6, 'u'), (1, 'p')]
	>>> compressed = compress('get uuuuuup')
	>>> ''.join(decompress(compressed))
	'get uuuuuup'

每当需要在数据上完成一个摘要的时候，都可以使用groupby。这时候内建的sorted函数
就非常有用，可以使传入的数据中相似的元素相邻。

##### (4). 其它函数

- [itertools函数完整列表](https://docs.python.org/2/library/itertools.html)

### 三、装饰器

装饰器原始的使用场景是可以将方法在定义的首部将其定义为类方法或静态方法。



### 四、with和contextlib

### 参考文档

- [Python在线教程](https://docs.python.org/2/tutorial/)

- [样式指南](http://www.python.org/dev/peps/pep-0008)