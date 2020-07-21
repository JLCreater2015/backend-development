# 计算机网络面试题

1、http请求头有哪些，http版本区别，http请求方式？【[链接](../network-communication-and-programming/agreement-forest/protocol-http.md#ban-ben-li-shi)】

2、get和post的区别。【[链接](../network-communication-and-programming/agreement-forest/protocol-http.md#3get-he-post)】

3、TCP三次握手和四次挥手。【[链接](../network-communication-and-programming/agreement-forest/protocol-tcp-udp.md#3tcp-lian-jie-he-shi-fang)】

* 为什么建立连接需要三次，断开连接为什么要四次？
* time\_wait 是在哪个步骤？
* 主动断开方的状态（FIN\_WAIT1，2，TIME\_WAIT）

> 三次握手：1、建立连接时，客户端发送SYN包（SYN=i）到服务器，并进入到SYN-SEND状态，等待服务器确认。2、服务器收到SYN包，必须确认客户的SYN（ack=i+1），同时自己也发送一个SYN包（SYN=k），即SYN+ACK包，此时服务器进入SYN-RECV状态。3、客户端收到服务器的SYN+ACK包，向服务器发送确认报ACK（ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手，客户端与服务器开始传送数据。
>
> 四次挥手：

4、TCP流量控制，拥塞控制。【链接】

5、TCP与UDP的区别。【链接】

6、ttl 是什么意思？IP头部的。【链接】

7、DNS 的解析过程。【链接】

8、TCP的可靠性传输是怎么保证的？【链接】

9、tcp的粘包问题。【链接】

10、http的常见状态码。【[链接](../network-communication-and-programming/agreement-forest/protocol-http.md#3-zhuang-tai-ma)】

11、浏览器的跨域问题。【链接】

12、session 和 cookie 的问题。【链接】

13、浏览器中输入 url 会发生什么？【[链接](../network-communication-and-programming/agreement-forest/protocol-http.md#gong-zuo-yuan-li)】

14、tcp中序号的作用。

15、http中的content-type表示什么？【[链接](../network-communication-and-programming/agreement-forest/protocol-http.md#qing-qiu-xiao-xi-request)】

16、Ddos攻击。

17、http是否无状态？如何有状态？session和Cookies的区别。

18、http2.0特性？介绍一下http2.0。【[链接](../network-communication-and-programming/agreement-forest/protocol-http.md#4-http-2)】

19、经过路由器的数据包的变化。

20、http和https有什么区别？【链接】

21、网络层和传输层是怎么区分的？

22、ping底层过程。

## 网络编程

1、多路复用 IO。【链接】

2、accept\(\)等待的时候，底层的原理是啥？有哪些数据结构？

3、服务端和客户端同时调用close\(\)会怎样？

4、为什么要使用反向代理？（1. 代理到常用的HTTPS端口 2. 解决跨域问题）

5、NIO的原理？

6、epoll的边缘触发和水平触发。



