## Advanced Python

## 使用zc.buildout

### 一、zc.buildout原理

virtualenv对于隔离一个python环境相当方便。zc.buildout提供了相同的隔离功能，并且进一步提供了：

* 一个简单的、在一个配置文件中定义这些依赖性的描述性语言

* 提供链接代码调用组合的输入点的插件系统

* 一种部署和发行应用程序源代码及其执行环境的方法

配置文件将描述环境中所需的egg，它们的状态以及构建应用程序所需要的所有其他元素

插件系统将注册这些包并按照执行的顺序将它们链接起来

最后，整个环境是独立和隔离的，因而可以像发行和部署后那样使用

#### 1. 配置文件结构

zc.buildout依赖于一个结构与ConfigParser模块兼容的配置文件，类似于ini配置文件。

#### (1) 最小的配置文件

最小的buildout配置文件中包含一个名为`[buildout]`的小节，其中有一个称作parts的变量。
这个变量包含一个提供小节列表的多行值，如下所示：

```ini
	[buildout]
	parts=
		part1
		part2
	[part1]
	recipe = my.recipe1
	[part2]
	recipe = my.recipe2
```

在parts中指定的每个小节至少有一个提供包名称的recipe值。这个包可以是任何python包，只要
它定义一个zc.buildout入口点。

用这个文件，buildout将执行以下工作序列：

* 检查my.recipe1包是否安装，如果没有安装，读取并完成本地安装

* 执行my.recipe1入口点所指向的代码

* 然后，对parts做同样的事情

因而，buildout是一个基于插件的脚本，将被称为recipe的独立包的执行链接起来。用这个工具构建的
环境包括recipe正确顺序的定义。

### 二、发行与分发

### 三、小结