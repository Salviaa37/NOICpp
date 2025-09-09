---
title: 函数参数传递
source: https://mp.weixin.qq.com/s/4w2HMLUQQqRK-5KFteCh7w
author:
  - "[[树超]]"
published: 
created: 2025-05-20
description: 
tags:
  - clippings
---
原创 树超 *2024年09月08日 08:00*
#Cpp 
嗨，又到一周一课的时候了，这次内容很简单：函数参数传递的类型。

### 函数参数传递的类型

C++有三种参数传递方式：

1. 按值传递（pass-by-value）
2. 按指针传递（pass-by-pointer）
3. 按引用传递（pass-by-reference）  
	其实，你也可以把“按指针传递”理解为“按值传递的特殊形式”，它本质上传递的是“指针的值”。而按引用传递是C++中提出来的，在C语言中是没有的。

  


### 举例子

下面我们通过实际三个函数来描述这三种传递方式，代码见我的GitHub <sup>[1]</sup> 。

```cpp
#include <iostream>
using namespace std;

// Pass by value
void func(int arg)
{
    cout << "arg = " << arg << endl;
    cout << "&arg =" << &arg << endl;
    arg = 5;
    cout << "arg = " << arg << endl;
}

// Pass by pointer
void func(int* arg)
{
    cout << "*arg = " << *arg << endl;
    cout << "arg =" << arg << endl;
    *arg = 5;
    cout << "*arg = " << *arg << endl;
}
// Pass by reference
void func_ref(int& arg)
{
    cout << "arg = " << arg << endl;
    cout << "&arg =" << &arg << endl;
    arg = 5;
    cout << "arg = " << arg << endl;
}
```

第一个函数是按值传递，特点是直接传递类型的值，该函数被调用时参数arg会保存参数的值。  
第二个函数是按指针传递，特点是传递参数的指针，该函数被调用时参数arg会保存参数的指针（的值）。  
第三个函数是按引用传递，特点是传递参数的引用，该函数被调用时参数arg会保存参数的引用。  
注：  
细心的你会留意到，第一个函数和第二个函数名相同，但是参数类型不同，这是C++中 函数重载 的用法，后面我们单独讲讲函数重载应用的注意事项。

### 跑程序

下面实际跑跑这3个函数，每个函数都传递同一个对象，但是输出结果会有不同。

```cpp
int main(void)
{
    // 按值传递：传递int类型
    int num = 47;
    cout << "=======pass-by-value===========" << endl;
    cout << "num = " << num << endl;
    cout << "&num = " << &num << endl;
    func(num);
    cout << "num = " << num << endl;
    
    // 按指传递，传递指针类型
    num = 47;
    cout << "=======pass-by-pointer===========" << endl;
    cout << "num = " << num << endl;
    cout << "&num = " << &num << endl;
    func(&num);
    cout << "num = " << num << endl;
    // 按值传递：传递引用类型
    num = 47;
    cout << "=======pass-by-reference===========" << endl;
    cout << "num = " << num << endl;
    cout << "&num = " << &num << endl;
    func_ref(num);
    cout << "num = " << num << endl;
    return 0;
}
```

输出结果：

```
=======pass-by-value===========
num = 47
&num = 0x16f4db328
arg = 47
&arg =0x16f4db2dc
arg = 5
num = 47
=======pass-by-pointer===========
num = 47
&num = 0x16f4db328
*arg = 47
arg =0x16f4db328
*arg = 5
num = 5
=======pass-by-reference===========
num = 47
&num = 0x16f4db328
arg = 47
&arg =0x16f4db328
arg = 5
num = 5
```

### 敲重点

下面来说说这三种类型传递的特别注意的地方。

1. 按值传递：无法修改参数的值、 也没有能力 改变参数的值。
1. 它顶多能改变argument的值，但是无法改变parameter的值（此处num为parameter，arg为argument）。
	2. 所以第一个函数arg的地址和num的地址是不相等的。
3. 按指针传递：可以通过指针解引用的方式，来修改参数（此处是num）的值。
1. 此处arg的值就是num的指针，所以你会在程序的输出两者相等。
	2. 然后num的值从47被修改为5。
5. 按引用传递：可以使用变量赋值的语法修改参数（此处是num）的值。
1. 与指针的区别是，不需要使用“指针解引用”的语法。
	2. 聪明的你，已经认识到：按引用传递本质是一种语法糖，是对”按指针传递“的另外一种语法表示。
	3. C++为何这样做呢？一者类似Java、C#、Python的函数参数传递默认就是传递的应用，另外这种语法的确可读性更强，作为不断的演进的语言，提供可读性更强的语法是很自然的事情。

### 特殊情况

在C语言中有一种类型叫 `void` ，尤其在函数传参时，C语言经常采用 `void *` 方式来传递一种未知类型的指针。  
用法举例：

```cpp
int i = 99;
void *vp = &i;
// *vp = 3 //编译错误
*((int*)vp) = 3 //必须强制类型转换后才能赋值
```

以上这种语法很难看，更糟糕的时 `void *` 这种语法在语言类型系统中引入了一个漏洞，允许开发者毫无顾及甚至非法的进让任意两个类型的指针互相转换。在C语言中，这称之为“强大、灵活、底层”，但是不推荐在C++中使用。后面我们会讲讲C++是如何进行类型转换的。

### 额外补充

现在，大家肯定已经理解了三种参数传递方式了。不过实际项目中，参数传递会有很多复杂问题。

1. 虽然按值传递可以达到“没有副作用”的效果，但若函数参数是一个占据空间比较大的C++对象，那么参数传递本身就会占用大量的计算资源。所以必须采用按指针传递、或者按引用传递。但是你并不想改变该对象，但是开发者总是会不小心改变该对象，怎么办？
2. 这个时候，需要在函数参数传递中引入const关键字：const关键字可以用来防止参数被修改，如果开发者不小心修改了编译器还会报错，这种用法明确的告诉了reader这个函数不会修改该对象。
3. 对于复合类型（结构体或类）的参数还需要考虑，这会触发“复制构造函数”。也就是说函数的参数会对该对象调用复制构造函数、然后执行完成后再调用析构函数。这不仅仅是性能的影响，若这个复合类型没有“正确的复制构造函数”还会引发SegmentFault。（何为正确的复制构造函数，以后单独开一课）

---

1. https://github.com/shuchaoma/cpp-illustrated/tree/main/ArgumentPassing ↩

  

