---
title: 一个实用的C++小知识（杂项运算符 1）
source: https://mp.weixin.qq.com/s?__biz=MzIxNTg1MjY2OA==&mid=2247484386&idx=1&sn=6900e12b24aac93fff13d67bc7d89bf8&scene=21#wechat_redirect
author:
  - "[[逝水流]]"
published: 
created: 2025-05-16
description: 运算符是一种告诉编译器执行特定的数学或逻辑操作的符号。
tags:
  - clippings
  - Cpp
---


原创 逝水流 [信息学奥赛一本通](https://mp.weixin.qq.com/) *2025年01月08日 16:13*


运算符是一种告诉编译器执行特定的数学或逻辑操作的符号。C++ 内置了丰富的运算符，并提供了以下类型的运算符：《算术运算符》、《关系运算符》、《逻辑运算符》、《位运算符》、《赋值运算符》、《杂项运算符》等。今天就让我们了解一下杂项运算符。

杂项运算符

| 运算符    | 描述                                                  | 实例                     |
| ------ | --------------------------------------------------- | ---------------------- |
| sizeof | 查看空间大小。                                             | cout<<sizeof(a);       |
| 三目     | 比较运算符。                                              | cout<<( a>b?a:b);      |
| ，      | 逗号运算符 会顺序执行一系列运算。整个逗号表达式的值是以逗号分隔的列表中的==最后一个表达式的值。== |                        |
| .和->   | 成员运算符 用于引用类、结构和共用体的成员。                              |                        |
| &      | 返回变量的地址。                                            | int a;  cout<<&a;      |
| \*     | 指向一个具体的变量                                           | int \*b=a;  cout<<\*b; |

## sizeof运算符

看下面程序：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){    
	int a=1;    
	//    查看int的空间大小   查看变量a的空间大小     
	cout<<sizeof(int)<<"   "<<sizeof(a)<<endl;     
	cout<<sizeof(long long)<<endl;    
	cout<<sizeof(float)<<endl;     
	cout<<sizeof(double)<<endl;     
	cout<<sizeof(char)<<endl;    
	cout<<sizeof(bool)<<endl; 
	    
	return 0;
}
```

程序输出：

| 数据类型      | 空间大小(字节) |
| --------- | -------- |
| int       | 4Byte    |
| long long | 8 Byte   |
| float     | 4 Byte   |
| double    | 8 Byte   |
| char      | 1 Byte   |
| bool      | 1 Byte   |

```CPp
4    4
8
4
8
1
1
```



## 三目运算符

看下面程序

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){    
	int a=10,b=5;    
	int c=(a>b?a:b);    
	cout<<(a>b?a:b)<<endl;     
	cout<<(a>b?"a大":"b大")<<endl;     
	cout<<c; 
	   
	return 0;
}
```

程序输出： 比较 ？的表达式，成立返回 ：前面的结果，不成立返回 ：后面的结果。
```cpp
10
a大
10
```


## 逗号运算符

看下面程序：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){    
	int a=1;
    cout<<a+3,a+1,a+6,a;//执行第1句 a+3    
    cout<<endl;     
    cout<<(a+3,a+1,a+6,a);//执行最后一句 a    
    return 0;
}
```

程序输出：

```Cpp
4
1
```

