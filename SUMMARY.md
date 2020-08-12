# Table of contents

* [Introduce](README.md)

## 操作系统和Linux <a id="operating-system"></a>

* [操作系统基础](operating-system/operating-system-basics/README.md)
  * [进程](operating-system/operating-system-basics/process.md)
  * [线程和协程](operating-system/operating-system-basics/thread-coroutine.md)
  * [进程间通信](operating-system/operating-system-basics/inter-process-communication.md)
  * [调度](operating-system/operating-system-basics/scheduling.md)
  * [死锁](operating-system/operating-system-basics/deadlock.md)
  * [内存管理](operating-system/operating-system-basics/memory-management.md)
  * [文件系统](operating-system/operating-system-basics/file-system.md)
  * [IO](operating-system/operating-system-basics/io.md)
* [Linux](operating-system/linux-basics/README.md)
  * [POSIX系统](operating-system/linux-basics/posix-system.md)
  * [系统调用](operating-system/linux-basics/system-calls.md)
  * [Linux共享内存](operating-system/linux-basics/shared-memory.md)
  * [Linux进程的内存空间布局](operating-system/linux-basics/linux-process-memory-space-layout.md)
  * [僵尸进程和孤儿进程](operating-system/linux-basics/zombie-orphan.md)
  * [用户态和内核态](operating-system/linux-basics/user-mode-and-kernel-mode.md)
  * [理解inode](operating-system/linux-basics/linux-inode.md)
* [Linux常考命令](operating-system/linux-command/README.md)
  * [管道和重定向](operating-system/linux-command/redirection-and-pipeline.md)
  * [三剑客之grep](operating-system/linux-command/command-grep.md)
  * [三剑客之sed](operating-system/linux-command/command-sed.md)
  * [三剑客之awk](operating-system/linux-command/command-awk.md)
  * [常用操作](operating-system/linux-command/common-operations.md)
* [Shell编程](operating-system/shell-programming.md)

## 网络通信与网络编程 <a id="network-communication-and-programming"></a>

* [计算机网络](network-communication-and-programming/computer-network/README.md)
  * [应用层其他协议](network-communication-and-programming/computer-network/protocol-application-layer.md)
  * [应用层之DNS协议](network-communication-and-programming/computer-network/application-layer-protocol-dns.md)
  * [应用层之HTTP/3协议](network-communication-and-programming/computer-network/application-layer-http3-quic.md)
  * [应用层之HTTPS协议](network-communication-and-programming/computer-network/application-layer-protocol-https.md)
  * [应用层之HTTP协议](network-communication-and-programming/computer-network/application-layer-protocol-http.md)
  * [传输层之UDP协议](network-communication-and-programming/computer-network/transport-layer-protocol-udp.md)
  * [传输层之TCP协议](network-communication-and-programming/computer-network/transport-layer-protocol-tcp.md)
  * [网络层其他协议](network-communication-and-programming/computer-network/protocol-ip-arp-icmp.md)
  * [网络层之IP协议](network-communication-and-programming/computer-network/network-layer-protocol-ip.md)
  * [数据链路层](network-communication-and-programming/computer-network/data-link-layer.md)
  * [物理层](network-communication-and-programming/computer-network/physical-layer.md)
* [网络编程](network-communication-and-programming/network-programming/README.md)
  * [cookie、session、token](network-communication-and-programming/network-programming/cookie-session-token.md)
  * [幂等性](network-communication-and-programming/network-programming/idempotence.md)
  * [网络IO模型](network-communication-and-programming/network-programming/network-io.md)
  * [网络编程底层代码示例](network-communication-and-programming/network-programming/underlying-code-example.md)
  * [TCP的粘包问题](network-communication-and-programming/network-programming/tcp-stick-packages.md)
  * [多路复用IO](network-communication-and-programming/network-programming/multiplexing-io.md)
* [Linux网络编程之深入源码](network-communication-and-programming/linux-network-source-code/README.md)
  * [传输控制块TCB](network-communication-and-programming/linux-network-source-code/transmission-control-block.md)
  * [TCP数据发送之tcp\_sendmsg\(\)](network-communication-and-programming/linux-network-source-code/tcp-send-tcp_sendmsg.md)
  * [TCP选项之MSS](network-communication-and-programming/linux-network-source-code/tcp-option-mss.md)
* [网络与安全](network-communication-and-programming/network-security.md)
* [Nginx](network-communication-and-programming/nginx-reverse-proxy.md)
* [Wireshark](network-communication-and-programming/wireshark.md)

## 数据库 <a id="database"></a>

* [数据库相关概念](database/database-concept.md)
* [关系数据库设计范式](database/relational-database-normal-form.md)
* [中级SQL](database/basic-sql-language.md)
* [高级SQL](database/advanced-sql-language.md)
* [Redis](database/redis/README.md)
  * [Redis数据类型](database/redis/redis-data-type.md)
  * [数据持久化](database/redis/data-persistence.md)
* [MySQL](database/mysql/README.md)
  * [MySQL数据类型](database/mysql/mysql-data-type.md)
  * [存储引擎](database/mysql/storage-engine.md)
  * [MyISAM与InnoDB](database/mysql/myisam-innodb.md)
  * [事务](database/mysql/transaction.md)
  * [锁机制](database/mysql/locks.md)
  * [索引](database/mysql/index.md)
  * [集群](database/mysql/cluster.md)
* [MongoDB](database/mongodb/README.md)
  * [查询](database/mongodb/select.md)
* [Memcached](database/memcached.md)

## 组成原理和体系结构

* [定点数 & 浮点数 & 内存](zu-cheng-yuan-li-he-ti-xi-jie-gou/fixed-floating-point-number.md)
* [体系结构](zu-cheng-yuan-li-he-ti-xi-jie-gou/architecture.md)

## 编译和调试 <a id="compile-and-debug"></a>

* [编译原理](compile-and-debug/compilers.md)
* [C++之静态库和动态库](compile-and-debug/static-and-dynamic-libraries.md)
* [Gdb调试](compile-and-debug/gdb-debug.md)
* [内存屏障](compile-and-debug/memory-barrier.md)
* [编译器优化](compile-and-debug/compiler-optimization.md)
* [make/Makefile](compile-and-debug/make-makefile.md)
* [cmake](compile-and-debug/cmake.md)
* [交叉编译](compile-and-debug/cross-compiling.md)
* [C++/Python混合编程](compile-and-debug/cpp-python-hybrid-programming.md)
* [C++/Lua混合编程](compile-and-debug/cpp-lua-hybrid-programming.md)

## 进程间通信

* [ZeroMQ](jin-cheng-jian-tong-xin/zeromq-message-queue.md)
* [Kafka](jin-cheng-jian-tong-xin/kafka.md)

## 分布式 & 微服务 & 云 <a id="distributed-microservices-cloud"></a>

* [Zookeeper](distributed-microservices-cloud/zookeeper.md)
* [分布式](distributed-microservices-cloud/distributed-system.md)
* [K8S](distributed-microservices-cloud/kubernetes.md)
* [Spring Cloud](distributed-microservices-cloud/spring-cloud.md)

## 其他 <a id="others"></a>

* [正则表达式](others/regular-expression/README.md)
  * [基本正则表达式](others/regular-expression/basic-regular-expression.md)
  * [扩展正则表达式](others/regular-expression/extended-regular-expression.md)
* [设计模式](others/design-patterns/README.md)
  * [单例模式](others/design-patterns/singleton-pattern.md)
* [虚拟技术](others/virtual-technology.md)
* [Git版本控制](others/git.md)
* [编码和字符集](others/code-character-set.md)
* [Vim用法](others/vim-usage.md)
* [一文解“锁”](others/lock-mechanism.md)
* [面试中的“锁”](others/locks.md)

## 面试题

* [计算机网络面试题](mian-shi-ti/net-interview-question.md)
* [操作系统面试题](mian-shi-ti/os-interview-questions.md)
* [数据库面试题](mian-shi-ti/database-interview-questions.md)
* [其他面试题](mian-shi-ti/other-interview-questions.md)
* [场景题总结](mian-shi-ti/scenario-questions.md)
* [智力题](mian-shi-ti/iq-test.md)

