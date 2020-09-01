# Linux进程调度算法



## ✏ 进程挂起

我们一般认为进程有五个状态，即新建态，就绪态，阻塞态，运行态，终止态。而在这些状态之外还存在着一个状态，我们称之为挂起状态，它既可以是我们客户主动使得进程挂起，也可以是操作系统因为某些原因使得进程挂起。总而言之引入挂起状态的原因有以下几种：

1. 用户的请求：可能是在程序运行期间发现了可疑的问题，需要暂停进程。 
2. 父进程的请求：考察，协调，或修改子进程。 
3. 操作系统的需要：对运行中资源的使用情况进行检查和记账。 
4. 负载调节的需要：有一些实时的任务非常重要，需要得到充足的内存空间，这个时候我们需要把非实时的任务进行挂起，优先使得实时任务执行。 
5. 定时任务：一个进程可能会周期性的执行某个任务，那么在一次执行完毕后挂起而不是阻塞，这样可以节省内存。 
6. 安全：系统有时可能会出现故障或者某些功能受到破坏，这是就需要将系统中正在进行的进程进行挂起，当系统故障消除以后，对进程的状态进行恢复。 

既然我们知道了挂起状态引入的原因，那么我们再来看看带有挂起状态的进程状态转移过程：

![](../../.gitbook/assets/image%20%2816%29.png)

相比于一般的五个状态的进程状态转移图，我们引入了两种挂起状态的类型，即就绪挂起状态和阻塞挂起状态。它们的区别就是就绪挂起状态其实还是在内存中的，而后者是在外存中的。接下来我们说一说新加入的几个状态转化的步骤：

1. 运行状态-&gt;就绪挂起状态：这里发生在客户在程序正在运行是直接挂起程序。注意这里的箭头是单向的，所以在就绪挂起状态结束以后实际上是执行激活步骤，进入就绪状态，等待处理机调度。 
2. 阻塞状态-&gt;阻塞挂起状态：当内存空间比较紧缺的时候，如果有存在内存中的，而且是处于阻塞状态的进程，那么就让他更需要内存的程序占用内存，自己进入阻塞挂起状态，PCB等数据存入外存。因为现在这个进程也不能进入就绪状态，这个程序在内存中是没有什么作用的。
3. 阻塞挂起状态-&gt;就绪挂起状态：当阻塞状态等待的IO事件或其他事件到来的时候状态发生改变。 
4. 就绪挂起状态-&gt;就绪状态：如果内存中没有就绪态进程，操作系统需要调入一个进程继续执行。此外，当处于就绪/挂起状态的进程比处于就绪态的任何进程的优先级都要高时，也可以进行这种转换。这种情况的产生是由于操作系统设计者规定，调入高优先级的进程比减少交换量更重要。 
5. 就绪状态-&gt;就绪挂起状态：通常，操作系统更倾向于挂起阻塞态进程而不是就绪态进程，因为就绪态进程可以立即执行，而阻塞态进程占用了内存空间但不能执行。但如果释放内存以得到足够空间的唯一方法是挂起一个就绪态进程，那么这种转换也是必需的。并且，如果操作系统确信高优先级的阻塞态进程很快就会就绪，那么它可能选择挂起一个低优先级的就绪态进程，而不是一个高优先级的阻塞态进程。

### 🖋 挂起和阻塞的区别

1. **是否释放CPU：**阻塞（pend）就是任务释放CPU，其他任务可以运行，一般在等待某种资源或信号量的时候出现。挂起（suspend）不释放CPU，如果任务优先级高就永远轮不到其他任务运行。一般挂起用于程序调试中的条件中断，当出现某个条件的情况下挂起，然后进行单步调试。
2. **是否主动：**显然阻塞是一种**被动行为**，其发生在**磁盘，网络IO，wait，lock**等要等待某种事件的发生的操作之后。因为拿不到IO资源，所以阻塞时会放弃 CPU的占用。而挂起是主动的，因为挂起后还要受到CPU的监督（等待着激活），所以挂起不释放CPU，比如sleep函数，站着CPU不使用。
3. **与调度器是否相关：**任务调度是操作系统来实现的，任务调度时，直接忽略挂起状态的任务，但是会顾及处于pend下的任务，当pend下的任务等待的资源就绪后，就可以转为ready了。ready只需要等待CPU时间，当然，任务调度也占用开销，但是不大，可以忽略。可以这样理解，只要是挂起状态，操作系统就不在管理这个任务了。

#### `sleep()`和`wait()`函数的区别：

1. **两者比较的共同之处是：两个方法都是使程序等待多少毫秒**。
2. **最主要区别是：sleep\(\)方法没有释放锁。而wait\(\)方法释放了锁，使得其他线程可以使用同步控制块或者方法**。
3. **sleep\(\)指线程被调用时，占着CPU不工作，形象的说明为“占着CPU”睡觉**。

### 🖋 进程主动挂起（sleep）

进程可以让自己挂起，等到一定时间后，被系统唤醒（时间到或者收到信号）。这个能力由sleep函数提供。

```c
unsigned int sleep(unsigned int seconds); 
```

这个函数可以让进程自己挂起seconds秒，sleep函数是由操作系统的 `nanosleep` 函数实现的，`On Linux, sleep() is implemented via nanosleep(2). See the nanosleep(2) man page for a discussion of the clock used.`核心代码：

```c
asmlinkage long sys_nanosleep(struct timespec __user *rqtp, 
                              struct timespec __user *rmtp)
{
	struct timespec t;
	unsigned long expire;
	long ret;

	expire = timespec_to_jiffies(&t) + (t.tv_sec || t.tv_nsec);
	current->state = TASK_INTERRUPTIBLE;
	expire = schedule_timeout(expire);
}
// 算出超时时间，然后挂起进程（可中断挂起），然后调用schedule_timeout。
```

```c
fastcall signed long __sched schedule_timeout(signed long timeout)
{
	struct timer_list timer;
	unsigned long expire;
	// 算出超时时间
	expire = timeout + jiffies;

	init_timer(&timer);
	// 超时时间
	timer.expires = expire;
	timer.data = (unsigned long) current;
	// 超时回调
	timer.function = process_timeout;
	// 添加定时器
	add_timer(&timer);
	// 进程调度
	schedule();
	// 删除定时器
	del_singleshot_timer_sync(&timer);
    // 超时或者被信号唤醒，被信号唤醒的话，可能还没有超时
	timeout = expire - jiffies;

out:
	return timeout < 0 ? 0 : timeout;
}
```

接着往系统新增一个定时器，然后发送进程调度，该进程随即进入挂起状态。等到一定的时间后，进程会唤醒。另外我们注意到挂起的进程状态是`TASK_INTERRUPTIBLE`，即可中断的。意思是这种状态的进程可以被信号唤醒。而`TASK_UNINTERRUPTIBLE`是不能被信号唤醒的，等到超时的时候，执行`process_timeout`函数：

```c
static void process_timeout(unsigned long __data)
{
	wake_up_process((task_t *)__data);
}
```

代码很简单，就是唤醒被挂起的进程。`__data`是在`timer.data = (unsigned long) current;` 中设置的。

