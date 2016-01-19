### 剖析TCP连接的状态变迁

![TCP连接的状态变迁图](tcp连接的状态变迁.png)

`CLOSED`：初始状态

`LISTEN`：服务器端的某个SOCKET处于监听状态，可以接受连接了

`SYN_RCVD`：表示接收到了SYN报文，在正常情况下，这个状态是服务器端的SOCKET在建立TCP连接
时的三次握手会话过程中的一个中间状态，很短暂，基本用netstat很难看到这种状态。因此，
这种状态时，当受到客户端的ACK报文后，它会进入到ESTABLISHED状态

`SYN_SENT`：这个状态与SYN_RCVD遥相呼应，当客户端SOCKET执行connect连接时，它首先发送
SYN报文，因此也就进入到了SYN_SENT状态，并等待服务器端的发送三次握手中的第2个报文。
SYN_SENT表示客户端已经发送SYN_SENT报文

`ESTABLISHED`：表示连接已经建立，即三次握手已完成

`FIN_WAIT_1`: 这个状态要好好解释一下，其实FIN_WAIT_1和FIN_WAIT_2状态的真正含义都是表示等待对方的FIN报文。
而这两种状态的区别是：FIN_WAIT_1状态实际上是当SOCKET在ESTABLISHED状态时，它想主动关闭连接，向对方发送了FIN报文，
此时该SOCKET即进入到FIN_WAIT_1状态。而当对方回应ACK报文后，则进入到FIN_WAIT_2状态，当然在实际的正常情况下，
无论对方何种情况下，都应该马上回应ACK报文，所以FIN_WAIT_1状态一般是比较难见到的，而FIN_WAIT_2状态还有时常常可以用netstat看到。

`FIN_WAIT_2`：上面已经详细解释了这种状态，实际上FIN_WAIT_2状态下的SOCKET，表示半连接，也即有一方要求close连接，但另外还告诉对方，
我暂时还有点数据需要传送给你，稍后再关闭连接。

`TIME_WAIT`: 表示收到了对方的FIN报文，并发送出了ACK报文，就等2MSL后即可回到CLOSED可用状态了。如果FIN_WAIT_1状态下，
收到了对方同时带FIN标志和ACK标志的报文时，可以直接进入到TIME_WAIT状态，而无须经过FIN_WAIT_2状态。
例外状态。正常情况下，当你发送FIN报文后，按理来说是应该先收到（或同时收到）对方的ACK报文，再收到对方的FIN报文。
但是`CLOSING`状态表示你发送FIN报文后，并没有收到对方的ACK报文，反而却也收到了对方的FIN报文。什么情况下会出现此种情况呢？
其实细想一下，也不难得出结论：那就是如果双方几乎在同时close一个SOCKET的话，那么就出现了双方同时发送FIN报文的情况，
也即会出现CLOSING状态，表示双方都正在关闭SOCKET连接。

`CLOSE_WAIT`: 这种状态的含义其实是表示在等待关闭。怎么理解呢？当对方close一个SOCKET后发送FIN报文给自己，你系统毫无疑问地会回应一个ACK报文给对方，
此时则进入到CLOSE_WAIT状态。接下来呢，实际上你真正需要考虑的事情是察看你是否还有数据发送给对方，如果没有的话，那么你也就可以close这个SOCKET，
发送FIN报文给对方，也即关闭连接。所以你在CLOSE_WAIT状态下，需要完成的事情是等待你去关闭连接。

`LAST_ACK`: 这个状态还是比较容易好理解的，它是被动关闭一方在发送FIN报文后，最后等待对方的ACK报文。当收到ACK报文后，也即可以进入到CLOSED可用状态了。
