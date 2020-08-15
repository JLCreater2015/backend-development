# 内存

## ✏ 1、`brk & sbrk`

**不属于Linux系统调用，而是C语言提供的。**

**函数原型：**

```c
#include<unistd.h>
int brk(void* addr); 
void* sbrk(intptr_t increment);
```

这两个函数都用来改变 “program break” \(程序间断点\)的位置，改变数据段长度（Change data segment size），即 **扩展heap的上界`brk`，**实现虚拟内存到物理内存的映射。 `brk()`函数直接修改有效访问范围的末尾地址实现分配与回收。当`addr`参数合理、系统有足够的内存并且不超过最大值时`brk()`函数将数据段结尾设置为`addr`，即间断点设置为`addr`。`sbrk()`函数中：当increment为正值时，间断点位置向后移动increment字节。同时返回移动之前的位置，相当于分配内存。当increment为负值时，位置向前移动increment字节，相当于释放内存，其返回值没有实际意义。当increment为0时，不移动位置只返回当前位置。参数increment的符号决定了是分配还是回收内存。

![](../../.gitbook/assets/image%20%281%29.png)

**返回值：**

`brk()`成功返回0，失败返回-1并且设置`errno`值为`ENOMEM`。 `sbrk()`成功返回之前的程序间断点地址。如果间断点值增加，那么这个指针（指的是返回之前的间断点地址）是指向分配的新的内存的首地址。如果出错失败，就返回一个指针并设置`errno`全局变量的值为`ENOMEM`。

### 测试

`sbrk()`与`brk()`是C标准函数的底层实现，其机制较为复杂（测试中，死循环是为了查看maps文件，不至于进程消亡文件随之消失）。

![](../../.gitbook/assets/image%20%282%29.png)

虽然，`sbrk()`与`brk()`均可分配回收兼职，但是我们一般用`sbrk()`分配内存，而用`brk()`回收内存，上例中回收内存可以这样写：

```c
int err = brk(old);
/**或者brk(p);效果与sbrk(-MAX*MAX);是一样的，但brk()更方便与清晰明了。**/
if(-1 == err){
    perror("brk");
    exit(EXIT_FAILURE);
}
```

## ✏ 2、`mmap & munmap`

 `mmap`函数第一种用法是映射磁盘文件到内存中，对该内存区域的存取即是直接对该文件内容的读写；而**`malloc`使用的`mmap`函数的第二种用法，即匿名映射，匿名映射不映射磁盘文件，而是向映射区申请一块内存**。

**函数原型：**

```c
#incldue<sys/mman.h>
void * mmap(void * addr, size_t length,int prot,int flags,int fd,off_t offset);

int munmap(void * addr, size_t length);
// addr为mmap函数返回接收的地址，length为请求分配的长度。
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">start</td>
      <td style="text-align:left">&#x6307;&#x5411;&#x6B32;&#x5BF9;&#x5E94;&#x7684;&#x5185;&#x5B58;&#x8D77;&#x59CB;&#x5730;&#x5740;&#xFF0C;&#x901A;&#x5E38;&#x8BBE;&#x4E3A;NULL&#xFF0C;&#x4EE3;&#x8868;&#x8BA9;&#x7CFB;&#x7EDF;&#x81EA;&#x52A8;&#x9009;&#x5B9A;&#x5730;&#x5740;&#xFF0C;&#x5BF9;&#x5E94;&#x6210;&#x529F;&#x540E;&#x8BE5;&#x5730;&#x5740;&#x4F1A;&#x8FD4;&#x56DE;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">length</td>
      <td style="text-align:left">&#x4EE3;&#x8868;&#x5C06;&#x6587;&#x4EF6;&#x4E2D;&#x591A;&#x5927;&#x7684;&#x90E8;&#x5206;&#x5BF9;&#x5E94;&#x5230;&#x5185;&#x5B58;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>prot</code>
      </td>
      <td style="text-align:left">
        <p>&#x4EE3;&#x8868;&#x6620;&#x5C04;&#x533A;&#x57DF;&#x7684;&#x4FDD;&#x62A4;&#x65B9;&#x5F0F;&#xFF0C;&#x6709;&#x4E0B;&#x5217;&#x7EC4;&#x5408;&#xFF1A;</p>
        <ul>
          <li><code>PROT_EXEC</code> &#x6620;&#x5C04;&#x533A;&#x57DF;&#x53EF;&#x88AB;&#x6267;&#x884C;&#xFF1B;</li>
          <li><code>PROT_READ</code> &#x6620;&#x5C04;&#x533A;&#x57DF;&#x53EF;&#x88AB;&#x8BFB;&#x53D6;&#xFF1B;</li>
          <li><code>PROT_WRITE</code> &#x6620;&#x5C04;&#x533A;&#x57DF;&#x53EF;&#x88AB;&#x5199;&#x5165;&#xFF1B;</li>
          <li><code>PROT_NONE</code> &#x6620;&#x5C04;&#x533A;&#x57DF;&#x4E0D;&#x80FD;&#x5B58;&#x53D6;&#x3002;</li>
        </ul>
        <p>&#x6CE8;&#x610F;<code>PROT_XXXXX</code>&#x4E0E;&#x6587;&#x4EF6;&#x672C;&#x8EAB;&#x7684;&#x6743;&#x9650;&#x4E0D;&#x51B2;&#x7A81;&#xFF0C;&#x5982;&#x679C;&#x5728;&#x7A0B;&#x5E8F;&#x4E2D;&#x4E0D;&#x8BBE;&#x5B9A;&#x4EFB;&#x4F55;&#x6743;&#x9650;&#xFF0C;&#x5373;&#x4F7F;&#x672C;&#x8EAB;&#x5B58;&#x5728;&#x8BFB;&#x5199;&#x6743;&#x9650;&#xFF0C;&#x8BE5;&#x8FDB;&#x7A0B;&#x4E5F;&#x4E0D;&#x80FD;&#x5BF9;&#x5176;&#x64CD;&#x4F5C;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">flags</td>
      <td style="text-align:left">
        <p>&#x4F1A;&#x5F71;&#x54CD;&#x6620;&#x5C04;&#x533A;&#x57DF;&#x7684;&#x5404;&#x79CD;&#x7279;&#x6027;&#xFF1A;</p>
        <ul>
          <li><code>MAP_FIXED</code> &#x5982;&#x679C;&#x53C2;&#x6570; start &#x6240;&#x6307;&#x7684;&#x5730;&#x5740;&#x65E0;&#x6CD5;&#x6210;&#x529F;&#x5EFA;&#x7ACB;&#x6620;&#x5C04;&#x65F6;&#xFF0C;&#x5219;&#x653E;&#x5F03;&#x6620;&#x5C04;&#xFF0C;&#x4E0D;&#x5BF9;&#x5730;&#x5740;&#x505A;&#x4FEE;&#x6B63;&#x3002;&#x901A;&#x5E38;&#x4E0D;&#x9F13;&#x52B1;&#x7528;&#x6B64;&#x65D7;&#x6807;&#x3002;</li>
          <li><code>MAP_SHARED</code> &#x5BF9;&#x5E94;&#x5C04;&#x533A;&#x57DF;&#x7684;&#x5199;&#x5165;&#x6570;&#x636E;&#x4F1A;&#x590D;&#x5236;&#x56DE;&#x6587;&#x4EF6;&#x5185;&#xFF0C;&#x800C;&#x4E14;&#x5141;&#x8BB8;&#x5176;&#x4ED6;&#x6620;&#x5C04;&#x8BE5;&#x6587;&#x4EF6;&#x7684;&#x8FDB;&#x7A0B;&#x5171;&#x4EAB;&#x3002;</li>
          <li><code>MAP_PRIVATE</code> &#x5BF9;&#x5E94;&#x5C04;&#x533A;&#x57DF;&#x7684;&#x5199;&#x5165;&#x64CD;&#x4F5C;&#x4F1A;&#x4EA7;&#x751F;&#x4E00;&#x4E2A;&#x6620;&#x5C04;&#x6587;&#x4EF6;&#x7684;&#x590D;&#x5236;&#xFF0C;&#x5373;&#x79C1;&#x4EBA;&#x7684;&quot;&#x5199;&#x5165;&#x65F6;&#x590D;&#x5236;&quot;
            (copy on write)&#x5BF9;&#x6B64;&#x533A;&#x57DF;&#x4F5C;&#x7684;&#x4EFB;&#x4F55;&#x4FEE;&#x6539;&#x90FD;&#x4E0D;&#x4F1A;&#x5199;&#x56DE;&#x539F;&#x6765;&#x7684;&#x6587;&#x4EF6;&#x5185;&#x5BB9;&#x3002;</li>
          <li><code>MAP_ANONYMOUS</code> &#x5EFA;&#x7ACB;&#x533F;&#x540D;&#x6620;&#x5C04;&#xFF0C;&#x6B64;&#x65F6;&#x4F1A;&#x5FFD;&#x7565;&#x53C2;&#x6570;<code>fd</code>&#xFF0C;&#x4E0D;&#x6D89;&#x53CA;&#x6587;&#x4EF6;&#xFF0C;&#x800C;&#x4E14;&#x6620;&#x5C04;&#x533A;&#x57DF;&#x65E0;&#x6CD5;&#x548C;&#x5176;&#x4ED6;&#x8FDB;&#x7A0B;&#x5171;&#x4EAB;&#x3002;</li>
          <li><code>MAP_DENYWRITE</code> &#x53EA;&#x5141;&#x8BB8;&#x5BF9;&#x5E94;&#x5C04;&#x533A;&#x57DF;&#x7684;&#x5199;&#x5165;&#x64CD;&#x4F5C;&#xFF0C;&#x5176;&#x4ED6;&#x5BF9;&#x6587;&#x4EF6;&#x76F4;&#x63A5;&#x5199;&#x5165;&#x7684;&#x64CD;&#x4F5C;&#x5C06;&#x4F1A;&#x88AB;&#x62D2;&#x7EDD;&#x3002;</li>
          <li><code>MAP_LOCKED</code> &#x5C06;&#x6620;&#x5C04;&#x533A;&#x57DF;&#x9501;&#x5B9A;&#x4F4F;&#xFF0C;&#x8FD9;&#x8868;&#x793A;&#x8BE5;&#x533A;&#x57DF;&#x4E0D;&#x4F1A;&#x88AB;&#x7F6E;&#x6362;(swap)&#x3002;</li>
        </ul>
        <p>
          <br />&#x5728;&#x8C03;&#x7528;<code>mmap()</code>&#x65F6;&#x5FC5;&#x987B;&#x8981;&#x6307;&#x5B9A;<code>MAP_SHARED</code> &#x6216; <code>MAP_PRIVATE</code>&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>fd</code>
      </td>
      <td style="text-align:left">open()&#x8FD4;&#x56DE;&#x7684;&#x6587;&#x4EF6;&#x63CF;&#x8FF0;&#x8BCD;&#xFF0C;&#x4EE3;&#x8868;&#x6B32;&#x6620;&#x5C04;&#x5230;&#x5185;&#x5B58;&#x7684;&#x6587;&#x4EF6;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">offset</td>
      <td style="text-align:left">&#x6587;&#x4EF6;&#x6620;&#x5C04;&#x7684;&#x504F;&#x79FB;&#x91CF;&#xFF0C;&#x901A;&#x5E38;&#x8BBE;&#x7F6E;&#x4E3A;0&#xFF0C;&#x4EE3;&#x8868;&#x4ECE;&#x6587;&#x4EF6;&#x6700;&#x524D;&#x65B9;&#x5F00;&#x59CB;&#x5BF9;&#x5E94;&#xFF0C;offset&#x5FC5;&#x987B;&#x662F;&#x5206;&#x9875;&#x5927;&#x5C0F;&#x7684;&#x6574;&#x6570;&#x500D;&#x3002;</td>
    </tr>
  </tbody>
</table>

> 返回值：失败返回`MAP_FAILED`，即`(void * (-1))`并设置`errno`全局变量。成功返回指向`mmap area`的指针`pointer`。
>
> 常见`errno`错误：

> **①`ENOMEM`**：内存不足；  
> **②`EAGAIN`**：文件被锁住或有太多内存被锁住；  
> **③`EBADF`**：参数`fd`不是有效的文件描述符；  
> **④`EACCES`**：存在权限错误，。如果是MAP\_PRIVATE情况下文件必须可读；使用MAP\_SHARED则文件必须能写入，且设置`prot`权限必须为`PROT_WRITE`。  
> **⑤`EINVAL`**：参数`addr`、length或者offset中有不合法参数存在。

范例：利用`mmap()`来读取`/etc/passwd` 文件内容：

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/mman.h>
main(){
    int fd;
    void *start;
    struct stat sb;
    fd = open("/etc/passwd", O_RDONLY); /*打开/etc/passwd */
    fstat(fd, &sb); /* 取得文件大小 */
    start = mmap(NULL, sb.st_size, PROT_READ, MAP_PRIVATE, fd, 0);
    if(start == MAP_FAILED) /* 判断是否映射成功 */
        return;
    printf("%s", start); 
    munmap(start, sb.st_size); /* 解除映射 */
    closed(fd);
}
```

在`mmap`和`munmap`执行过程的任何时刻，被映射文件的`st_atime`可能被更新。若`st_atime`字段在上述情况下没有得到更新，首次对映射区的第一个页索引时会更新该字段的值。

使用`PROT_WRITE` 和 `MAP_SHARED`标志建立起来的文件映射，其`st_ctime(c: change)` 和 `st_mtime(m: modification)` 在对映射区写入之后与`msync()[memory synchronize]`通过`MS_SYNC` 和 `MS_ASYNC[memory asynchronize]`两个标志调用之前会被更新。

测试`framebuffer`的小程序：显示红色方块。（对`Framebuffer`进行读写可以直接操作显存，该程序需要在Linux的命令行模式下运行。）

```c
#include <unistd.h>  
#include <stdio.h>  
#include <stdlib.h>  
#include <fcntl.h>  
#include <linux/fb.h>  
#include <sys/mman.h>  

int main()
{
    int fp = 0;
    struct fb_var_screeninfo  vinfo;
    struct fb_fix_screeninfo  finfo;
    long screensize = 0;
    char* fbp = 0;
    int x = 0, y = 0;
    long location = 0;
    fp = open("/dev/fb0", O_RDWR);
    sleep(10);
    if (fp < 0)
    {
        printf("Error : Can not open framebuffer device\n");
        exit(1);
    }
    if (ioctl(fp, FBIOGET_FSCREENINFO, &finfo))
    {
        printf("Error reading fixed information\n");
        exit(2);
    }
    if (ioctl(fp, FBIOGET_VSCREENINFO, &vinfo))
    {
        printf("Error reading variable information\n");
        exit(3);
    }

    screensize = vinfo.xres * vinfo.yres * vinfo.bits_per_pixel / 8;
    /*这就是把fp所指的文件中从开始到screensize大小的内容给映射出来，得到一个指向这块空间的指针*/
    fbp = (char*)mmap(0, screensize, PROT_READ | PROT_WRITE, MAP_SHARED, fp, 0);
    if ((int)fbp == -1)
    {
        printf("Error: failed to map framebuffer device to memory./n");
        exit(4);
    }
    /*这是你想画的点的位置坐标,(0，0)点在屏幕左上角*/
    x = 100;
    y = 100;

    for (x = 100; x < 200; x++)
    {
        for (y = 100; y < 200; y++)
        {
            location = x * (vinfo.bits_per_pixel / 8) + y * finfo.line_length;
            *(fbp + location) = 0;  /* 蓝色的色深 */  /*直接赋值来改变屏幕上某点的颜色*/
            *(fbp + location + 1) = 0; /* 绿色的色深*/
            *(fbp + location + 2) = 200; /* 红色的色深*/
            *(fbp + location + 3) = 0;  /* 是否透明*/
        }
    }

    munmap(fbp, screensize); /*解除映射*/
    close(fp);    /*关闭文件*/
    return 0;
}

```

