# 计算机网络面试题

1、http请求头有哪些，http版本区别，http请求方式？【[链接](../network-communication-and-programming/computer-network/application-layer-protocol-http.md#ban-ben-li-shi)】

2、get和post的区别。【[链接](../network-communication-and-programming/computer-network/application-layer-protocol-http.md#3get-he-post)】

3、TCP三次握手和四次挥手。【[链接](../network-communication-and-programming/computer-network/transport-layer-protocol-tcp.md#3tcp-lian-jie-he-shi-fang)】

* 为什么建立连接需要三次，断开连接为什么要四次？
* time\_wait 是在哪个步骤？
* 主动断开方的状态（`FIN_WAIT1/2`，`TIME_WAIT`）

> 三次握手：1、建立连接时，客户端发送SYN包（SYN=i）到服务器，并进入到SYN-SEND状态，等待服务器确认。2、服务器收到SYN包，必须确认客户的SYN（`ack=i+1`），同时自己也发送一个SYN包（SYN=k），即`SYN+ACK`包，此时服务器进入`SYN-RECV`状态。3、客户端收到服务器的`SYN+ACK`包，向服务器发送确认报`ACK（ack=k+1）`，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手，客户端与服务器开始传送数据。
>
> 四次挥手：

4、TCP流量控制，拥塞控制。【[链接](../network-communication-and-programming/computer-network/transport-layer-protocol-tcp.md#7tcp-ke-kao-xing-zhi-liu-liang-kong-zhi)】

5、TCP与UDP的区别。【[链接](../network-communication-and-programming/computer-network/transport-layer-protocol-udp.md#4udp-he-tcp-de-qu-bie)】

6、TTL 是什么意思？IP头部的。【链接】

7、DNS 的解析过程。【链接】

8、TCP的可靠性传输是怎么保证的？【[链接](../network-communication-and-programming/computer-network/transport-layer-protocol-tcp.md#5tcp-ke-kao-xing-zhi-bao-ying-da-xu-lie-hao)】

9、TCP的粘包问题。【链接】

10、HTTP的常见状态码。【[链接](../network-communication-and-programming/computer-network/application-layer-protocol-http.md#3-zhuang-tai-ma)】

11、浏览器的跨域问题。【链接】

12、session 和 cookie 的区别。【链接】

13、浏览器中输入 URL 会发生什么？【[链接](../network-communication-and-programming/computer-network/application-layer-protocol-http.md#gong-zuo-yuan-li)】

14、TCP中序号的作用。【[链接](../network-communication-and-programming/computer-network/transport-layer-protocol-tcp.md#5tcp-ke-kao-xing-zhi-bao-ying-da-xu-lie-hao)】

15、HTTP中的content-type表示什么？【[链接](../network-communication-and-programming/computer-network/application-layer-protocol-http.md#qing-qiu-xiao-xi-request)】

16、Ddos攻击。

17、http是否无状态？如何有状态？

18、http2.0特性？介绍一下http2.0。【[链接](../network-communication-and-programming/computer-network/application-layer-protocol-http.md#4-http-2)】

19、经过路由器的数据包的变化。

20、http和https有什么区别？【链接】

21、网络层和传输层是怎么区分的？

22、ping底层过程。

23、TCP三次握手成功后拔掉网线，过一会儿再插上网线，这个过程中会发生什么？（爱奇艺面试题）

24、什么是半连接队列？

25、 ISN\(Initial Sequence Number\)是固定的吗？ 

26、三次握手过程中可以携带数据吗？ 

27、如果第三次握手丢失了，客户端服务端会如何处理？ 

28、SYN攻击是什么？ 

29、挥手为什么需要四次？ 

30、四次挥手释放连接时，等待`2MSL`的意义?

31、HTTP的长链接机制。

32、HTTPS的传输过程。

## 网络编程

1、多路复用 IO。【链接】

2、`accept()`等待的时候，底层的原理是啥？有哪些数据结构？

3、服务端和客户端同时调用`close()`会怎样？

4、为什么要使用反向代理？（1. 代理常用的`HTTPS`端口 2. 解决跨域问题）

5、`NIO`的原理？

6、`epoll`的边缘触发和水平触发。

7、怎么判断对方要断开链接？



