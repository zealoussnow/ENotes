### [FastCGI/Fast Common Gateway Interface](https://zh.wikipedia.org/wiki/FastCGI)

一种让交互程序与Web服务器通信的协议，是CGI的增强版本。它致力于减少网页服务器与CGI程序之间的互动开销，
从而使服务器能处理更多的网页请求。

实现：与为每个请求创建一个新的进程不同，FastCGI使用持续的进程处理一连串的请求，这些请求由FastCGI服务器
管理，而不是web服务器。当进来一个请求时，web服务器把环境变量和这个页面请求通过一个socket比如FastCGI进程
与web服务器(都位于本地）或者一个TCP connection（FastCGI进程在远端的server farm）传递给FastCGI进程。


### [REST/Representational State Transfer](https://zh.wikipedia.org/wiki/REST)

目前在三种主流的Web服务实现方案中，因为REST模式与复杂的SOAP和XML-RPC相比更加简洁，越来越多的web服务
开始采用REST风格设计和实现。

REST是设计风格而不是标准。REST通常基于使用HTTP，URI，和XML以及HTML这些现有的广泛流行的协议和标准：

* 资源是由URI来指定。

* 对资源的操作包括获取、创建、修改和删除资源，这些操作正好对应HTTP协议提供的GET 、 POST 、 PUT和DELETE方法。

* 通过操作资源的表现形式来操作资源。

* 资源的表现形式则是XML或者HTML，取决于读者是机器还是人，是消费web服务的客户软件还是web浏览器。当然也可以是
任何其他的格式。

REST是含状态传输，应该注意区别应用的状态和连接协议的状态。HTTP连接是无状态的（也就是不记录每个连接的信息），
而REST传输会包含应用的所有状态信息，因此可以大幅降低对HTTP连接的 重复请求 资源消耗。

### Python资源

[Writing Forwards Compatible Python Code](http://lucumr.pocoo.org/2011/1/22/forwards-compatible-python/)
