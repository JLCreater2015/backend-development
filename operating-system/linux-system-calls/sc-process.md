# 进程

## ✏ 1、`fork & vfork`

### 🖋 1、函数fork\(\)

fork函数：创建一个新进程

1. fork\(\)成功后，将为子进程申请PCB和用户内存空间。
2. 子进程会复制父进程用户空间的所有数据\(代码段、数据段、BSS、堆、栈\)，文件描述符。
3. 复制父亲进程PCB中绝大多数信息。
4. 虽然子进程复制了文件描述符，而对于文件描述符相关的文件表项\(struct file结构\)，则采用共享的方式。这也是父子进程进行pipe互相通信的基础。

```cpp
#include<sys/types.h>
#include<unistd.h>
#include<stdio.h>
 
int main()
{
    int count = 0;
    pid_t pid;
    pid = fork();
    if(pid < 0)
        printf("error in fork!\n");
    else if(pid == 0)
        printf("I am the child process,ID is %d,count is %d(%p)\n",
               getpid(),count,&count);
    else 
        printf("I am the parent process,ID is %d,count is %d(%p)\n",
               getpid(),count,&count);
    return 0;
}

// 输出
I am the parent process,ID is 5188,count is 0(0x7ffd5e311580)
I am the child process,ID is 5189,count is 1(0x7ffd5e311580)
```

fork\(\)函数用于从已存在的进程中创建一个新的进程，新的进程称为子进程，而原进程称为父进程，**fork\(\)的返回值有两个，子进程返回0，父进程返回子进程的进程号，进程号都是非零的正整数，所以父进程返回的值一定大于零。**子进程与父进程count的地址（虚拟地址）是相同的（但他们在内核中被映射的物理地址不同）。

**因此，`fork()`函数的和返回值有三种情况：0，子进程id，小于0（-1）。**

### 🖋 2、copy-on-write

第一代Unix系统中创建进程：当发出fork\(\)系统调用时，内核原样复制父进程的整个地址空间并把复制的那一份分配给子进程。这种行为是非常耗时的，因为它需要：

* 为子进程的页表分配页帧
* 为子进程的页分配页帧
* 初始化子进程的页表
* 把父进程的页复制到子进程相应的页中

这种创建地址空间的方法涉及许多内存访问，消耗许多CPU周期，并且完全破坏了高速缓存中的内容。在大多数情况下，这样做常常是毫无意义的，因为许多子进程通过装入一个新的程序开始它们的执行，这样就完全丢弃了所继承的地址空间。

现在的Linux内核采用一种更为有效的方法，称之为写时复制（Copy On Write，COW）。这种思想相当简单：父进程和子进程共享页帧而不是复制页帧。然而，只要页帧被共享，它们就不能被修改，即页帧被保护。无论父进程还是子进程何时试图写一个共享的页帧，就产生一个异常，这时内核就把这个页复制到一个新的页帧中并标记为可写。原来的页帧仍然是写保护的：当其他进程试图写入时，内核检查写进程是否是这个页帧的唯一属主，如果是，就把这个页帧标记为对这个进程是可写的。

起初子进程复制了父进程的task\_struct，系统堆栈空间和页面表，子进程会拥有与父进程相同的物理页面。为了节约内存和加快创建速度的目标，fork\(\)函数会让子进程以只读方式共享父进程的物理页面，同时将父进程对这些物理页面的访问权限也设成只读。这样，当父进程或子进程任何一方对这些已共享的物理页面执行写操作时，都会产生页面出错异常\(`page_fault int14`\)中断，此时CPU会执行系统提供的异常处理函数`do_wp_page()`来解决这个异常。`do_wp_page()`会对这块导致写入异常中断的物理页面进行取消共享操作，为写进程复制一新的物理页面，使父进程和子进程各自拥有一块内容相同的物理页面。最后，从异常处理函数中返回时，CPU就会重新执行刚才导致异常的写入操作指令，使进程继续执行下去。

上面的例子中，我们没有执行`++count`前，其实子进程和父进程的count指向的是同一块内存。而当子进程改变了变量时候（即对变量进行了写操作），会通过copy\_on\_write的手段为所涉及的页面建立一个新的副本。所以当我们执行`++count`后，这时候子进程才新建了一个页面复制原来页面的内容，基本资源的复制是必须的，而且是高效的。

**写入时复制\(Copy-on-write\)**是一个被使用在程式设计领域的最佳化策略。其基础的观念是，如果有多个呼叫者\(callers\)同时要求相同资源，他们会共同取得相同的指标指向相同的资源，直到某个呼叫者\(caller\)尝试修改资源时，系统才会真正复制一个副本\(private copy\)给该呼叫者，以避免被修改的资源被直接察觉到，这过程对其他的呼叫都是通透的\(transparently\)。此作法主要的优点是如果呼叫者并没有修改该资源，就不会有副本\(private copy\)被建立。

### 🖋 3、`vfork`函数

`vfork`创建的子进程与父进程共享地址空间，即子进程完全运行在父进程的地址空间上，子进程对虚拟地址空间的修改同样为父进程所见。一般情况下，用`vfork`函数创建子进程后，子进程往往要调用一种exec函数以执行另一个程序，当进程调用一种exec函数时，该进程完全由新程序代换，而新程序则从其main函数开始执行，因为调用exec并不创建新进程，所以前后的进程id 并未改变，exec只是用另一个新程序替换了当前进程的正文，数据，堆和栈段。不过在子进程中调用exec或exit之前，他在父进程的空间中运行。

```cpp
#include<sys/types.h>
#include<unistd.h>
#include<stdio.h>

int main()
{
    pid_t pid;
    int cnt = 0;
    pid = vfork();
    if(pid<0)
        printf("error in fork!\n");
    else if(pid == 0)
    {
        cnt++;
        printf("cnt=%d\n",cnt);
        printf("I am the child process,ID is %d\n",getpid());
        _exit(0); // _exit(0);
    }
    else if(pid > 0)
    {
        cnt++;
        printf("cnt=%d\n",cnt);
        printf("I am the parent process,ID is %d\n",getpid());
    }
    return 0;
}

// 输出
cnt=1
I am the child process,ID is 19713
cnt=2
I am the parent process,ID is 19712

// 如果注释掉 __exit(0)，则会出现一下错误：
cnt=1
I am the child process,ID is 13986
cnt=-134746042
I am the parent process,ID is 13985
test_vfork: cxa_atexit.c:100: __new_exitfn: Assertion `l != NULL' failed.
[1]    13985 abort (core dumped)  ./test_vfork
```

### 🖋 4、区别

1. `fork()`的子进程拷贝父进程的数据段和代码段； `vfork()`的子进程与父进程共享数据段；
2. `fork()`的父子进程的执行次序不确定； `vfork()`保证子进程先运行，在调用 `exec` 或 `exit` 之

   前与父进程数据是共享的，在它调用 `exec` 或 `exit` 之后父进程才可能被调度运行；

3. `vfork()`保证子进程先运行，在它调用 exec 或 exit 之后父进程才可能被调度运行。如果

   在调用这两个函数之前子进程依赖于父进程的进一步动作，则会导致死锁。

## ✏ 2、exec家族

## ✏ 3、`exit & __exit`

## ✏ 4、`wait & waitpid`

## ✏ 5、clone

系统调用`fork()`和`vfork()`是无参数的，而`clone()`则带有参数。**`fork()`是全部复制，`vfork()`是共享内存，而clone\(\)是则可以将父进程资源有选择地复制给子进程**，而没有复制的数据结构则通过指针的复制让子进程共享，具体要复制哪些资源给子进程，由参数列表中的clone\_flags决定。

