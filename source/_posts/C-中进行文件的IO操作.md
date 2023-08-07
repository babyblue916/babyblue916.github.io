---
title: C++中进行文件的IO操作
date: 2023-08-07 17:31:06
tags: C++
---

C++中通过对文件的操作保存文本或二进制数据。

对文件操作需要包含头文件**<fstream>**

# 1.文本文件

## 1.写文件

其操作步骤如下：

1.包含头文件

```c++
#include <fstream>
```

2.创建流对象

```c++
ofstream ofs;
```

3.打开文件

```c++
ofs.open("文件路径", 文件打开方式);
```

4.写数据

```c++
ofs << "写入的数据";
```

5.关闭文件

```c++
ofs.close();
```

第三步中文件的打开方式有以下几种：

|  打开方式   |             解释             |
| :---------: | :--------------------------: |
|   ios::in   |            读文件            |
|  ios::out   |            写文件            |
|  ios::ate   |     初始位置定义为文件尾     |
|  ios::app   |        追加方式写文件        |
| ios::trunc  | 如果文件存在，先删除，再创建 |
| ios::binary |          二进制方式          |

文件打开方式可以多个配合使用，方式间利用 | 操作符隔开。

例如：用二进制方式写文件：

```c++
 ios::out | ios::binary
```







## 2.读文件

读文件步骤如下：

1.包含头文件

```c++
#include <fstream>
```

2.创建流对象

```c++
ifstream ifs;
```

3.打开文件并判断文件是否打开成功

```
ifs.open("文件路径", 打开方式);
```

4.读数据（四种）

```c++
//第一种方式：
char a[1024] = {0};
while(ifs >> a)
{
	cout << a << endl;
}
//第二种:
char a[1024] = {0};
while(ifs.getline(a, sizeof(a)))
{
    cout << a << endl;
}
//第三种：
string a;
while(getline(ifs, a))
{
    cout << a << endl;
}
//第四种：
char a;
while((a = ifs.get()) != EOF)
{
    cout << a;
}
```

5.关闭文件

```c++
ifs.close();
```







# 2.二进制文件

## 1.写文件

二进制方式写文件主要利用流对象调用成员函数**write**

函数原型：

```c++
ostream& write(const char * buffer, int len);
```

其中，字符指针buffer指向内存中的一段存储空间，len是读写的字节数。

示例代码：

1.包含一个Person类

```c++
class Person{
public:
	char m_Name[64];
	int m_Age;
};
```

2.测试函数test()

```c++
void test()
{
	//创建输出流对象同时打开文件
    ofstream ofs("Person.txt", ios::out | ios::binary);
    Person p = {"张三", 18};
    //写文件
    ofs.write((const char*)&p, sizeof(p));
    //关闭文件
    ofs.close();
}
```

3.主函数

```c++
int main()
{
	test();
	system("pause");
	return 0;
}
```







## 2.读文件

二进制方式读文件主要利用流对象调用成员函数**read**

函数原型：`istream& read(char *buffer,int len);`

参数解释：字符指针buffer指向内存中一段存储空间，len是读写的字节数。

示例代码：

1.包含一个Person类

```c++
class Person{
public:
	char m_Name[64];
	int m_Age;
};
```

2.测试函数test()

```c++
void test()
{
	//创建输入流对象同时打开文件
    ifstream ifs("Person.txt", ios::in | ios::binary);
    if(!ifs.is_open())
    {
        cout << "文件打开失败！" << endl;
    }
    Person p = {"张三", 18};
    //读文件
    ifs.read((char*)&p, sizeof(p));
    cout << "姓名： " << p.m_Name << " 年龄： " << p.m_Age << endl;
    //关闭文件
    ofs.close();
}
```

3.主函数

```c++
int main()
{
	test();
	system("pause");
	return 0;
}
```



