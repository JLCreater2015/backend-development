# 网络IO模型

## ✏ 1、网络IO

网络IO的本质就是socket流的读取，socket在`linux`系统被抽象为流，IO可以理解为对流的操作，通常一次IO读操作会涉及到两个对象和两个阶段。

两个对象分别是：

* 用户进程（线程）Process（Thread） 
* 内核对象 Kernel 

两个阶段：

* 等待流数据准备（wating for the data to be ready）; 
* 从内核向进程复制数据（copying the data from the kernel to the process）; 

对于socket流而已：

* 第一步通常涉及等待网络上的数据分组到达，然后被复制到内核的某个缓冲区。 
* 第二步把数据从内核缓冲区复制到应用进程缓冲区。 

Richard Stevens的经典书籍`UNP`，书中给出5种IO模型：

* blocking IO - 阻塞IO 
* non-blocking IO - 非阻塞IO 
* IO multiplexing - IO多路复用 
* signal driven IO - 信号驱动IO 
* asynchronous IO - 异步IO 

