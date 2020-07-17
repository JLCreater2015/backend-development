---
description: 总结C++标准IO库的功能和使用方法。
---

# IO

C++标准库提供的IO操作涉及到的头文件有：`<ios>`、`<istream>`、`<ostream>`、`<streambuf>`、`<iostream>`、`<fstream>`、`<sstream>`。

## ✏ 类结构

C++标准库中一共有三种IO类，标准输入输出\(`iostream`\)，文件输入输出\(`fstream`\)，字符串输入输出\(`sstream`\)，具体类型如下图，其中以`w`开头的类型是宽字符版本，`sstream`和`fstream`继承自`iostream`，所以这些类型之间有着共同的特性。

![](../../.gitbook/assets/50.gif)

* `ios_base`：表示流的基本特征；
* `ios`：继承于`ios_base`，提供了一个指向`streambuf`的指针；
* `streambuf`：为缓冲区提供了内存，并提供了用于操作缓冲区的方法; 
* `istream/wistream`：继承于`ios`类，提供了输入方法；
* `ostream/wostream`：继承于`ios`类，提供了输出方法；
* `iostream/wiostream`：继承于`istream`和`ostream`，提供了输入输出方法；
* `ifstream/wifstream`：继承于`istream`，提供了对文件进行输入的方法；
* `ofstream/wofstream`：继承于`ostream`，提供了对文件进行输出的方法；
* `fstream/wfstream`：继承于`iostream`，提供了对文件进行输入输出的方法；
* `istringstream/wistringstream`：继承于`istream`，对字符串进行操作的输入流类；
* `ostringstream/wostringstream`：继承于`ostream`，对字符串进行操作的输出流类；
* `stringstream/wstringstream`：继承于`iostream`，对字符串进行操作的输入输出流类。

## ✏ 标准IO

### 🖋 1、常见IO对象

C++中标准库提供了四个标准IO对象:

1. `cin`：标准输入流对象,默认情况下关联到标准输入设备\(键盘\)。 
2. `cout`：标准输出流对象，默认情况下关联到标准输出设备\(显示器\)。 
3. `clog`：表示错误的标准输出流对象，默认情况下关联到标准输出设备\(显示器\)。 
4. `cerr`：用于日志记录的标准输出流对象，默认情况下关联到标准输出设备\(显示器\)。该流未被缓冲，这意味这信息将直接发送给屏幕，而不是等到缓冲区中填满数据或有新的换行符才会发送。 

> 实际上应有8个对象，针对宽字符`wchar_t`，以上4个对象都有对应的处理`wchar_t`的对象。

#### 🖌 1.1、IO对象无拷贝或赋值

流对象不能存储在vector或其他容器中。形参或返回类型也不能为流类型，如果需要，则必须传递或返回指向该对象的指针或引用，而且不能是`const`引用，因为读写一个IO对象会改变其状态。

#### 🖌 1.2、IO条件状态

有5个状态标志位：

1. `ios_base::iostate` ：一种机器相关的类型，提供表达条件状态的完整功能。
2. `ios_base::badbit` ：用来标识流已经崩溃，无法挽回。
3. `ios_base::failbit`：用来标识一个IO操作失败了。
4. `ios_base::eofbit`：用来指出流到达了文件结束
5. `ios_base::goodbit`：用来指出流未处于错误状态，此值保证为0。

查询流状态的接口：

1. `s.eof()` ：若流 s 的`eofbit`置位，返回`true`
2. `s.fail()` ：若流 s 的`failbit`或`badbit`置位，返回`true`
3. `s.bad()` ：若流 s 的`badbit`置位，返回`true`
4. `s.good()` ：若流s处于有效状态，返回`true`
5. `s.clear()` ：将流 s 中所有的条件状态位复位，将流的状态设置为有效，返回`void`
6. `s.clear(flags)` ：根据给定的`flags`标志位，将流 s 的对应条件状态位复位，`flags`的类型为`strm::iostate`，返回`void`
7. `s.setstate(flags)` ：根据给定的`flags`标志位，将流 s 中对应的条件状态位置位，`flags`类型位`strm::iostate` 返回`void`
8. `s.rdstate()` 返回流 s 当前的条件状态，返回值类型为 `strm::iostate`

### 🖋 2、输入

> 从键盘输入字符串的时候需要敲一下回车键才能够将这个字符串送入到缓冲区中，那么敲入的这个回车键\(`\r`\)会被转换为一个换行符`\n`，这个换行符`\n`也会被存储在`cin`的缓冲区中并且被当成一个字符来计算。`cin`读取数据是从输入缓冲区中获取数据，缓冲区为空时，`cin`的成员函数会阻塞等待数据的到来，一旦缓冲区中有数据，就触发`cin`的成员函数去读取数据。

**使用`cin`进行读取输入，常用的读取方法有:**

* `operator >>`：用于读取满足参数的类型\(基本数据类型都可以\)，读取时将忽略空白符号；

当`cin>>`从缓冲区中读取数据时，若缓冲区中第一个字符是空格、`tab`或换行这些分隔符时，`cin>>`会将其忽略并清除，继续读取下一个字符，若缓冲区为空，则继续等待。但是如果读取成功，字符后面的分隔符是残留在缓冲区的，`cin>>`不做处理。不想略过空白字符，可以使用 `noskipws` 流控制。比如`cin >> noskipws >> input`。

* `int get()`：读取单个字符，读取到文件尾时，返回`EOF`；
* `istream& get(char& c)`：读取单个字符到c，读取到文件尾时，返回false；
* `istream& get(char s, streamsize n)`：用于读取C风格字符串，它不会丢弃换行符，换行符将继续留在输入缓冲区中，因此接下来的输入操作首先将读取换行符；
* `istream& get(char* s, streamsize n, char delim)`：同上，但可指定结束符；
* `istream& get(streambuf& sb)`；
* `istream& get(streambuf& sb, char delim)`；

> 其中`streamsize` 在`VC++`中被定义为long long型。

* `istream& getline(char s, streamsize n)`：用于读取C风格字符串，通过换行符来确定行尾，但不保存换行符，而是将换行符用空字符来替换；
* `istream& getline(char* s, streamsize count, char delim)`；

`cin.getline`与`cin.get`的区别是，`cin.getline`不会将结束符或者换行符残留在输入缓冲区中。

#### 🖌 2.1、输入错误时的处理

`ios_base`中包含一个用来描述流状态的数据成员，由3个标记位组成：

* `ios_base::eofbit`：当`cin`操作到达文件末尾时，设置此标记位；
* `ios_base::failbit`：当`cin`未能读取到预期字符时，设置此标记位；
* `ios_base::badbit`：当存在无法诊断的失败破坏流时，设置此标记位。

以上任何一个如果被设置，那么流将对后面的输入/输出关闭，直到位被清除。`clear()`方法负责重置标记位。 

```cpp
int score;
for (int i=0;i < 10;i++) {
    std::cout << "please enter your score: ";
    while(!(std::cin >> score)){
        std::cin.clear(); //clear state bit
        while(std::cin.get() != '\n') { // 清空输入队列中的错误字符
            continue;
        }
        std::cout << "please enter a number: ";
        continue;
    }
    std::cout << "your score: " << score << std::endl;
}
// 当输入数据不能转换为int时读操作就会失败，cin的failbit就会置位。
// 6-8行，如果不清空错误字符，4行的while会一直进入。
```

#### 🖌 2.2、清空输入缓冲区

#### 🖌 2.3、其他输入的方式

**1、`getline`读取一行**

C++中定义了一个在std名字空间的全局函数`getline()`，因为这个函数的参数使用了`string`字符串，所以声明在了`<string>`头文件中。`getline()`利用`cin`可以从标准输入设备键盘读取一行，当遇到如下三种情况会结束读操作：1）到文件结束；2）遇到函数的定界符；3）输入达到最大限度。

函数原型有两个重载形式：

```cpp
istream& getline(istream& is, string& str);  //默认以换行符结束
istream& getline(istream& is, string& str, char delim);
```

### 🖋 3、输出





## ✏ 文件IO

## ✏ 字符串IO





