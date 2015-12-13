### REST(Representational State Transfer)

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
