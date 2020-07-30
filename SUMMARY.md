# Table of contents

* [Introduce](README.md)

## C++与STL <a id="cpp-stl"></a>

* [C++标准库](cpp-stl/cpp-standard-library/README.md)
  * [IO](cpp-stl/cpp-standard-library/io-library.md)
  * [Tuple](cpp-stl/cpp-standard-library/pair-tuple.md)
  * [String类](cpp-stl/cpp-standard-library/string-class.md)
  * [regex](cpp-stl/cpp-standard-library/regex.md)
* [STL深入源码](cpp-stl/stl-source-analysis.md)
* [QT Quick与QML](cpp-stl/qt-quick-qml.md)
* [QT Widgets](cpp-stl/qt-widgets.md)
* [第三方库](cpp-stl/third-party-libraries/README.md)
  * [JsonCpp](cpp-stl/third-party-libraries/jsoncpp.md)

## 操作系统和Linux <a id="operating-system"></a>

* [操作系统基础](operating-system/operating-system-basics/README.md)
  * [进程](operating-system/operating-system-basics/process.md)
  * [线程](operating-system/operating-system-basics/thread.md)
  * [进程间通信](operating-system/operating-system-basics/inter-process-communication.md)
  * [调度](operating-system/operating-system-basics/scheduling.md)
  * [死锁](operating-system/operating-system-basics/deadlock.md)
  * [内存管理](operating-system/operating-system-basics/memory-management.md)
  * [文件系统](operating-system/operating-system-basics/file-system.md)
  * [IO](operating-system/operating-system-basics/io.md)
* [Linux基础](operating-system/linux-basics/README.md)
  * [POSIX信号处理](operating-system/linux-basics/posix-signal.md)
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
  * [物理层](network-communication-and-programming/computer-network/physical-layer.md)
  * [数据链路层](network-communication-and-programming/computer-network/data-link-layer.md)
* [网络编程](network-communication-and-programming/network-programming/README.md)
  * [cookie、session、token](network-communication-and-programming/network-programming/cookie-session-token.md)
  * [网络IO模型](network-communication-and-programming/network-programming/network-io.md)
  * [网络编程底层代码示例](network-communication-and-programming/network-programming/underlying-code-example.md)
  * [TCP的粘包问题](network-communication-and-programming/network-programming/tcp-stick-packages.md)
  * [多路复用IO](network-communication-and-programming/network-programming/multiplexing-io.md)
* [网络与安全](network-communication-and-programming/network-security.md)
* [Linux网络编程之深入源码](network-communication-and-programming/linux-network-source-code/README.md)
  * [传输控制块TCB](network-communication-and-programming/linux-network-source-code/transmission-control-block.md)
  * [TCP数据发送之tcp\_sendmsg\(\)](network-communication-and-programming/linux-network-source-code/tcp-send-tcp_sendmsg.md)
  * [TCP选项之MSS](network-communication-and-programming/linux-network-source-code/tcp-option-mss.md)
* [协议森林](network-communication-and-programming/agreement-forest/README.md)
  * [DNS](network-communication-and-programming/agreement-forest/protocol-dns.md)
  * [HTTP](network-communication-and-programming/agreement-forest/protocol-http.md)
  * [HTTPS](network-communication-and-programming/agreement-forest/protocol-https.md)
  * [HTTP/3](network-communication-and-programming/agreement-forest/http3-quic.md)
  * [TCP](network-communication-and-programming/agreement-forest/protocol-tcp.md)
  * [UDP](network-communication-and-programming/agreement-forest/protocol-udp.md)
  * [IP](network-communication-and-programming/agreement-forest/protocol-ip.md)
  * [网络层协议](network-communication-and-programming/agreement-forest/protocol-ip-arp-icmp.md)
  * [应用层协议](network-communication-and-programming/agreement-forest/protocol-application-layer.md)
* [Nginx](network-communication-and-programming/nginx-reverse-proxy.md)

## 数据库 <a id="database"></a>

* [数据库相关概念](database/database-concept.md)
* [关系数据库设计范式](database/relational-database-normal-form.md)
* [SQL语句](database/sql-language.md)
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

* [体系结构](zu-cheng-yuan-li-he-ti-xi-jie-gou/architecture.md)

## 编译和调试 <a id="compile-and-debug"></a>

* [编译原理](compile-and-debug/compilers.md)
* [C++之静态库和动态库](compile-and-debug/static-and-dynamic-libraries.md)
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

## 分布式

* [Zookeeper](fen-bu-shi/zookeeper.md)
* [分布式](fen-bu-shi/distributed-system.md)

## 其他 <a id="others"></a>

* [正则表达式](others/regular-expression/README.md)
  * [基本正则表达式](others/regular-expression/basic-regular-expression.md)
  * [扩展正则表达式](others/regular-expression/extended-regular-expression.md)
* [虚拟技术](others/virtual-technology.md)
* [Git版本控制](others/git.md)
* [设计模式](others/design-patterns.md)
* [编码和字符集](others/code-character-set.md)

## 面试题

* [面试中的“锁”](mian-shi-ti/locks.md)
* [C++面试题总结](mian-shi-ti/cpp-interview-question.md)
* [计算机网络面试题](mian-shi-ti/net-interview-question.md)
* [操作系统面试题](mian-shi-ti/os-interview-questions.md)
* [数据库面试题](mian-shi-ti/database-interview-questions.md)
* [其他面试题总结](mian-shi-ti/other-interview-questions.md)
* [场景题总结](mian-shi-ti/scenario-questions.md)

