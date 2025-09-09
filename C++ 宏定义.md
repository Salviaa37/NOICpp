宏定义


## [C++中的宏定义](https://www.cnblogs.com/fnlingnzb-learner/p/6903966.html)

在C++中，宏定义是一种预处理器指令，用于在编译之前将宏名替换为相应的替换文本。宏定义可以提高程序的运行效率和可读性，但也需要谨慎使用，以避免潜在的错误。

无参宏定义

无参宏定义是最简单的宏定义形式，用于将一个标识符替换为一个字符串。例如：

```cpp
#define PI 3.1415926
```

在程序中，所有出现的_PI_都会被替换为_3.1415926_[1](https://blog.csdn.net/qq_32584715/article/details/112940881)。

带参宏定义

带参宏定义允许在宏中使用参数，从而实现更复杂的替换。例如：

```cpp
#define SQUARE(x) ((x)*(x))
```

在使用时，_SQUARE(3+2)_会被替换为_((3+2)*(3+2))_，从而避免了运算优先级的问题[2](https://blog.csdn.net/Dontla/article/details/122526092)。

宏中的特殊符号

宏定义中可以使用_#_和_##_符号来进行字符串化和连接操作。例如：

```
#define STR(s) #s

#define CONS(a, b) int(a##e##b)
```

_STR(hello)_会被替换为_"hello"_，而_CONS(2, 3)_会被替换为_int(2e3)_[1](https://blog.csdn.net/qq_32584715/article/details/112940881)。

多行宏定义

多行宏定义可以使用反斜杠_\\_来连接多行。例如：
```

#define DOIT(m, n) for(int i=0; i<(n); ++i)\\

{\\

m += i;\\

}

```
这样可以在宏中定义一个完整的循环[1](https://blog.csdn.net/qq_32584715/article/details/112940881)。

宏的优缺点

宏的优点是可以提高程序的执行效率，因为宏在预处理阶段就被展开，避免了函数调用的开销。然而，宏也有一些缺点，例如容易出错，难以调试等[2](https://blog.csdn.net/Dontla/article/details/122526092)。

总之，宏定义是C++中一个强大的工具，但在使用时需要谨慎，确保宏的定义和使用不会引入潜在的错误。

[1](https://blog.csdn.net/qq_32584715/article/details/112940881): [C++中的宏定义详解_c++宏定义常量-CSDN博客](https://blog.csdn.net/qq_32584715/article/details/112940881) [2](https://blog.csdn.net/Dontla/article/details/122526092): [c/c++宏函数的定义与使用（宏定义函数）（macro definition）_c++宏定义函数-CSDN博客](https://blog.csdn.net/Dontla/article/details/122526092)




```cpp
#define SUM(x,y) x+y

int a=3;
int b=2;
a+=a*SUM(a,b)*b;
```

答案是32，因为宏定义是单纯的替换关系，不关心代码实际含义；

替换后的代码是 a*a+b*b，即3*3+2*2

```cpp

int main(){
    int x=1, y=1, z=1;
    x *= y + 8;
    cout << x <<' ';
    z = z*(y+8);
    cout << z <<' ';
    return 0;

}

```

答案是一样的，因为复合赋值运算符先计算，再赋值。
```cpp

int main(){
    int a=b=c=0;
    a=b=c=10;
    cout<<a<<" "<<b<<" "<<c<<endl;
    return 0;
}

```
编译错误，C/C++规定必须先声明，再使用，b、c在使用前并未声明，会遇到编译器报错