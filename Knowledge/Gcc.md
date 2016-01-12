### gcc编译选项

* -Wl,-Bstatic 	指定优先链接静态库

* -Wl,-Bdynamic 指定优先链接动态库

* -m32 在64位系统中生成32位的程序，系统中必须要存在32位的库

* -Wl,-rpath,$HOME/third-party/lib 指定运行时寻找的第三方库的路径
