# The Linux Command

#### ifconfig -

1.显示本地公网IP

	$ curl http://ifconfig.me
	$ curl ifconfig.me
	$ 124.207.226.122 [result]

#### MAKEDEV - create device

#### ss - a utility to investigate sockets

#### locate - find files by name
2.使用locate命令高效查找文件

	$ locate -w "*.c"

更新mlocate.db。通常系统会在固定时间自动更新这个文件，也可以使用updatedb命令手动进行更新。

	$ sudo updatedb

The data base is /var/cache/locate/locatedb in Debian system and /var/lib/mlocate/mlocate.db 
in Ubuntu system and I don't know other system

#### addr2line - convert address into file names and line numbers

    addr2line -e ./a.out [address]
    /home/zealoussnow/workspace/test/hello.c:7
