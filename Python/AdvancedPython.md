## Advanced Python

### 语法最佳实践——低于类级

#### 1. 列表推导(List Comprehensions)

	[ i for i in range(10) if i % 2 == 0]

每当要对序列中的内容进行循环处理时，就应该尝试用List Comprehensions来代替它。

#### 2. 迭代器和生成器(Iterator & Generator)

迭代器只不过是一个实现迭代器协议的容器对象，它基于两个方法：

* next 返回容器的下一个项目

* \_\_iter\_\_ 返回迭代器本身

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

#### 参考文档

- [Python在线教程](https://docs.python.org/2/tutorial/)

- [样式指南](http://www.python.org/dev/peps/pep-0008)
