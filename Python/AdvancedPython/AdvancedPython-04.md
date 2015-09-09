## Advanced Python

## 编写一个包

### 一、用于所有包的公共模块

setup.py模板：

	from setuptools import setup
	setup(name='tools.name')

name给出了egg的全名，由此，该脚本提供多个命令，可以使用--help-commands选项列出这些
命令。

Standard command是distutils提供的内建命令，而Extra commands是由诸如setuptools
这样的第三方包或任何其他定义和注册新命令的包所创建的。

#### 1. sdist

sdist命令是最简单的命令，它用来创建一个发行树，运行一个包所需的一切内容都将复制到这里。
然后，这棵树被归档到一个或多个档案文件中。这个档案基本上是一个源树的副本。

这个命令是从目标系统独立地分发一个包的最简单方式。它将创建一个dist文件夹，其中包含可被
分发的档案。使用它之前，必须传递一个附加的参数给setup，以提供版本号，如果不给它提供一个
version值，那么它将使用version=0.0.0。

	from setuptools import setup
	setup(name='tools.name', version='0.1.1')

这个版本号在升级时十分有用，每当发行包时都将提升版本号，这样目标系统就知道它已经被修改了。

在sdist分发中，如果包含有C程序库或扩展，目标系统将负责编译它们，这在基于Linux和Mac OS
的系统上是很常见的，因为他们都提供编译器。但是，在windows下并不常见。这就是当一个包打算
在多个平台下运行时，应该总是和一个预编译分发版本一起分发的原因。



#### 2. MANIFEST.in文件

### 二、基于模板的方法

### 三、创建包模板

### 四、开发周期

### 五、小结

<hr>

[下一章：编写一个应用程序](./AdvancedPython-05.md)