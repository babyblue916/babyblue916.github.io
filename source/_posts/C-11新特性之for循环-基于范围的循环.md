---
title: C++11新特性之for循环(基于范围的循环)
date: 2023-08-30 10:50:07
tags: C++
categories: 学习
---

# For基于范围的循环

C++ 11标准之前（C++ 98/03 标准），如果要用 for 循环语句遍历一个数组或者容器，只能套用如下结构：

```c++
for(表达式 1; 表达式 2; 表达式 3){
  //循环体
}
```

例如，下面程序演示了用上述结构遍历数组和容器的具体实现过程：

```c++
#include <iostream>
#include <vector>
#include <string.h>
using namespace std;
int main() 
{    char arc[] = "www.crazymorty.com";
    int i;
    //for循环遍历普通数组
    for (i = 0; i < strlen(arc); i++) {
        cout << arc[i];
    }
    cout << endl;
    vector<char>v(arc,arc+18);
    vector<char>::iterator it;
    //for循环遍历 vector 容器
    for (it = v.begin(); it != v.end(); ++it) {
        cout << *it;
    }
    return 0;
}
```

程序执行结果为：

```
www.crazymorty.com
www.crazymorty.com
```

[^其中vector是STL标准库提供的序列式容器]: 

而 C++ 11 标准中，除了可以沿用前面介绍的用法外，还为 for 循环添加了一种全新的语法格式，如下所示：

```c++
for (declaration : expression){
  //循环体
}
```



其中，两个参数各自的含义如下：

- declaration：表示此处要定义一个变量，该变量的类型为要遍历序列中存储元素的类型。需要注意的是，C++ 11 标准中，declaration参数处定义的变量类型可以用 auto 关键字表示，该关键字可以使编译器自行推导该变量的数据类型。
- expression：表示要遍历的序列，常见的可以为事先定义好的普通数组或者容器，还可以是用 {} 大括号初始化的序列。

可以看到，同 C++ 98/03 中 for 循环的语法格式相比较，此格式并没有明确限定 for 循环的遍历范围，这是它们最大的区别，即旧格式的 for 循环可以指定循环的范围，而 C++11 标准增加的 for 循环，只会逐个遍历 expression 参数处指定序列中的每个元素。

下面程序演示了如何用C++11标准中的for循环：

```c++
#include <iostream>
#include <vector>
using namespace std;
int main() 
{    char arc[] = "www.crazymorty.com";
    int i;
    //for循环遍历普通数组
    for(char ch : arc){
        cout << ch;
    }
    cout << "!" << endl;
    vector<char>v(arc,arc+18);
    for(auto x : v){
        cout << x;
    }
 	cout << "!" << endl;
    return 0;
}
```

程序输出结果为

```c++
www.crazymorty.com !
www.crazymorty.com!
```

其中注意两点：

- 程序中定义了auto类型的x，当编译器编译程序时，会自动根据vector容器中定义的类型推导出x所需要的类型为char。
- 观察输出结果，其中第一行输出的字符串和 "!" 之间还输出有一个空格，这是因为新格式的 for 循环在遍历字符串序列时，不只是遍历到最后一个字符，还会遍历位于该字符串末尾的 '\0'（字符串的结束标志）。之所以第二行输出的字符串和 "!" 之间没有空格，是因为  v  容器中没有存储 '\0'。 

在使用新语法格式的 for 循环遍历某个序列时，如果需要遍历的同时修改序列中元素的值，实现方案是在 declaration 参数处定义引用形式的变量。举个例子：

```c++
#include <iostream>
#include <vector>
using namespace std;
int main() {
    char arc[] = "abcde";
    vector<char>v(arc, arc + 5);
    //for循环遍历并修改容器中各个字符的值
    for (auto &ch : v) {
        ch++;
    }
    //for循环遍历输出容器中各个字符
    for (auto ch : v) {
        cout << ch;
    }
    return 0;
}
```

程序输出结果为：

```c++
bcdef
```

