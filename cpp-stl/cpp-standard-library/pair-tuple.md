# Tuple

## ✏ Pair

> pair类型定义在 **\#include &lt;utility&gt;** 头文件中，定义如下：

```cpp
//类模板：
template<class T1,class T2> struct pair
```

> 参数：T1是第一个值的数据类型，T2是第二个值的数据类型。
>
> 功能：pair将一对值\(T1和T2\)组合成一个值；这一对值可以具有不同的数据类型（T1和T2）；两个值可以分别用pair的**两个公有函数first和second访问**。

```cpp
std::pair<std::string, std::string> getAuthor() {
    return std::make_pair("Zhang", "Liu");
}

void testPair(){
    // 1、创建pair对象
    std::pair<int, std::string> p1(10, "hello");
    std::pair<int, std::string> p2(p1);
    std::pair<int, std::string> p3;
    p3 = p2;

    typedef std::pair<std::string, std::string> Author;
    Author author1("Tom", "Jack");

    // 2、元素的操作
    std::cout << "origin:" << p1.first << "  " << p1.second << std::endl;
    p1.first = 2;
    p1.second = "world";
    std::cout << "modify:" << p1.first << "  " << p1.second << std::endl;

    // 3、生成新的对象
    p1 = std::make_pair(5, "Hello World!");
    std::cout << "final:" << p1.first << "  " << p1.second << std::endl;

    // 4、对象间运算
    std::cout << (p1 == p2) << std::endl;
    std::cout << (p1 > p2) << std::endl;
    // 两个pair对象间的小于运算，其定义遵循字典次序：
    // 如 p1.first < p2.first 或者 !(p2.first < p1.first) && (p1.second < p2.second) 则返回true。
    std::cout << (p1 < p2) << std::endl;

    // 5、使用tie获取pair元素值
    // 在某些清况函数会以pair对象作为返回值时，可以直接通过std::tie进行接收。
    std::string name1,name2;
    std::tie(name1, name2) = getAuthor();
    std::cout << "Authors:" << name1 << "  " << name2 << std::endl;
}
```

## ✏ Tuple/tie\(\) --C++11

> tuple容器\(元组\)定义在 \#include &lt;tuple&gt; 头文件中，它是 pair 的泛化，是表示元组容器，是不包含任何结构的，快速而低质（quick and dirty）的，可以用于函数返回多个返回值；
>
> 可以使用直接初始化，和 "make\_tuple\(\)" 初始化，**访问元素使用 "get&lt;&gt;\(\)" 方法**，注意get里面的位置信息，必须是常量表达式\(const expression\)；
>
> 可以通过 ****"std::tuple\_size&lt;decltype\(t\)&gt;::value" ****获取元素数量和 "std::tuple\_element&lt;0, decltype\(t\)&gt;::type" 获取元素类型；如果tuple类型进行比较，则需要保持元素数量相同，类型可以比较，如相同类型，或可以相互转换类型（int&double）；
>
> 无法通过普通的方法遍历tuple容器，因为 "get&lt;&gt;\(\)" 方法无法使用变量获取值。

```cpp

```

> 全局的 tie&lt;&gt;\(\) 函数模板定义在 &lt;tuple&gt; 头文件中，它提供了另一种访问 tuple 元素的方式。这个函数可以把 tuple 中的元素值转换为可以绑定到 tie&lt;&gt;\(\) 的左值集合。tie&lt;&gt;\(\) 的模板类型参数是从函数参数中推导的。

