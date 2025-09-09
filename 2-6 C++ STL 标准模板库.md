---
source: https://ivanclf.github.io/2024/04/26/C++%20STL/
created: 2025-06-20
tags:
  - clippings
---
## 前言

**C++ 标准模板库 (STL, Standard Template Library)** ：包含一些常用数据结构与算法的模板的 C++ 软件库。其包含四个组件——算法 (Algorithms)、容器 (Containers)、仿函数 (Functors)、迭代器 (Iterators).  
示例：

- 算法： `sort(a.begin(), a.end())`
- 容器： `priority_queue<int> pque`
- 仿函数： `greater<int>()`
- 迭代器： `vector<int>::iterator it = a.begin()`

STL 是 C++ 标准库的一部分，使用STL时需要`#include`对应的头文件。

STL 作为一个封装良好，性能合格的 C++ 标准库，在算法竞赛中运用极其常见。灵活且正确使用 STL 可以节省非常多解题时间，这一点不仅是由于可以直接调用，还是因为它封装良好，可以让代码的可读性变高，解题思路更清晰，调试过程 ~~往往~~ 更顺利。  

不过 STL 毕竟使用了==很多复杂的结构来实现丰富的功能==，它的效率往往是比不上自己手搓针对特定题目的数据结构与算法的。因此，STL 的使用相当于使用更长的运行时间换取更高的编程效率。因此，在实际比赛中要权衡 STL 的利弊，不过这一点就得靠经验了。  

以下重点讲解在算法竞赛中常用的 STL 容器和算法，对于函数和迭代器，就不着重展开讲了。

## STL 容器

### STL 向量 vector

头文件
**`#include <vector>`**  

`<vector>` 是 STL 中的一个容器类，用于==存储动态大小的数组==。

`<vector>` 是一个序列容器，它允许用户在容器的末尾快速地添加或删除元素。与数组相比，`<vector>` 提供了更多的功能，如自动调整大小、随机访问等。

简单来说，STL中的向量 vector是一个动态数组，在编程中常用于线性表（序列、栈、队列）的模拟与实现，是使用最多的STL容器，需要完全掌握其用法。

#### 常用方法

##### 声明与初始化

**`vector<类型> arr(长度, [初值])`**  
时间复杂度： $O(n)$  
常用的一维和二维数组构造示例，高维也是一样的（就是会有点长）.

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> arr;         // 构造int数组
vector<int> arr(100);    // 构造初始长100的int数组
vector<int> arr(100, 1); // 构造初始长100的int数组，每个元素初值为1
vector<double> name;
vector<char> name;
vector<struct node> name;

int arr[10][10];
/*多维数组 */
vector<vector<int>> name; // 构造一个二维数组
vector<vector<int>> mat(100, vector<int> ());       // 构造初始100行，不指定列数的二维数组
vector<vector<int>> mat(100, vector<int> (666, -1)) // 构造初始100行，初始666列的二维数组，初值为-1
vector<int> arr[100];         // 正确，构造初始100行，不指定列数的二维数组，可用于链式前向星存图
```

##### 增加和删除
`.push_back(元素)` ：在 vector 尾接一个元素，数组长度 $+1$ .
 `.pop_back()` ：删除 vector 尾部的一个元素，数组长度 $−1$
时间复杂度：O(1)

```cpp
// init: arr = []
arr.push_back(1);
// after: arr = [1]
arr.push_back(2);
// after: arr = [1, 2]
arr.pop_back();
// after: arr = [1]
arr.pop_back();
// after: arr = []
```

##### 通过索引访问元素
和一般数组一样的作用  
时间复杂度：O(1)
```cpp
int firstElement = arr[0];
```

##### 获取长度与改变长度
**`.size()`**  
获取当前 vector 的长度  
时间复杂度：O(1)
```cpp
vectorz<int> arr(1000);
size_t size = arr.size(); //size的值是1000
```

**`.resize(新长度, [新元素默认值])`**  
修改 vector 的长度
- 如果是缩短，则删除多余的值
- 如果是扩大，且指定了默认值，则新元素均为默认值 **（旧元素不变）** 
时间复杂度：O(n)

##### 清空与判空
**`.clear()`**  ：清空 vector  
**`.empty()`**  ：如果是空返回 `true` 反之返回 `false`
时间复杂度：O(1)
```cpp
// 清空
arr.clear();

// 判空
if (arr.empty()) {
	std::cout << "The vector is now empty." << std::endl;
}
```

##### 遍历
和数组一样用for循环遍历
```cpp
for (int i = 0; i < arr.size(); i++){
    cout << a[i] << endl;
}
```




#### 适用情形

==一般情况 `vector` 可以替换掉普通数组==，除非该题卡常。

普通数组没法解决情况：如 $n*m$ 大小的矩阵， 且
- 如果用普通数组 `int mat[1000010][1000010]` ，浪费内存，会导致 MLE。
- 如果使用 `vector<vector<int>> mat(n + 10, vector<int> (m + 10))` ，完美解决该问题。  
	另外， `vector` 的数据储存在堆空间中，不会栈溢出。

#### 注意事项

- 提前指定长度
  如果长度已经确定，那么应当==直接在构造函数指定长度==，而不是一个一个 `.push_back()`. 因为 `vector` 额外内存耗尽后的==重分配是有时间开销的==，直接指定长度就不会出现重分配了。
```cpp
// 优化前的运行时长: 522ms
vector<int> arr;
for (int i = 0; i < 1e8; i++)
    arr.push_back(i);


// 优化后的运行时长: 259ms
vector<int> arr(1e5); // 1e5是科学计数法，表示1*10^5,即100000
for (int i = 0; i < a.size(); i++)
    arr[i] = i;
```

- 当心` size_t` 溢出
  vector 获取长度的方法 `.size()` 返回值类型为 `size_t` ，通常 OJ 平台使用的是 32 位编译器（有些平台例如 cf 可选 64 位），那么该类型范围为 .
```cpp
vector<int> a(65536);
long long a = a.size() * a.size(); // 直接溢出变成0了
```


### STL 链表 list

 `<list>` 容器用于存储元素集合，支持双向迭代器（相当于双向链表）。

`<list>` 是 C++ 标准模板库（STL）中的一个序列容器，它允许在容器的==任意位置快速插入和删除元素==。与向量`<vector>`（动态数组）不同，`<list>` 不需要在创建时指定大小，并且可以==在任何位置添加或删除元素==，而不需要重新分配内存。


list的特点
- **双向迭代**：`<list>` 提供了双向迭代器，可以向前和向后遍历元素。
- **动态大小**：与数组不同，`<list>` 的大小可以动态变化，不需要预先分配固定大小的内存。
- **快速插入和删除**：可以在列表的任何位置快速插入或删除元素，而不需要像向量那样移动大量元素。
#### 常用方法

##### 声明与初始化
```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> lst1;                  // 空的list
    list<int> lst2(5);               // 包含5个默认初始化元素的list
    list<int> lst3(5, 10);           // 包含5个元素，每个元素为10
    list<int> lst4 = {1, 2, 3, 4};   // 使用初始化列表

    return 0;
}
```

##### 常见操作
- 声明列表：`list<T> mylist;`，其中 `T` 是存储在列表中的元素类型。
- 插入元素：`mylist.push_back(value);`
- 删除元素：`mylist.pop_back();` 或 `mylist.erase(iterator);`
- 访问元素：`mylist.front();` 和 `mylist.back();`
- 遍历列表：使用迭代器 `for (auto it = mylist.begin(); it != mylist.end(); ++it)`

示例
```cpp
#include <iostream>
#include <list>

int main() {
    // 创建一个整数类型的列表
    list<int> lst;

    // 向列表中添加元素
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);

    // 访问并打印列表的第一个元素
    cout << "First element: " << lst.front() << endl;

    // 访问并打印列表的最后一个元素
    cout << "Last element: " << lst.back() << endl;

    // 遍历列表并打印所有元素
    cout << "List elements: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;

    // 删除列表中的最后一个元素
    lst.pop_back();

    // 再次遍历列表并打印所有元素
    cout << "List elements after removing the last element: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;

    return 0;
}

```
#### 适用情形
list类似于链表，适合频繁增删元素，不需要经常访问元素的场景。

#### 注意事项
- `<list>` 的元素是==按插入顺序存储==的，而不是按元素值排序。
- 由于 `<list>` 的元素==存储在不同的内存位置==，就像链表一样，所以它不适合需要随机访问的场景。
- 与向量相比，`<list>` 的内存使用效率较低，因为每个元素都需要额外的空间来存储指向前后元素的指针。



### STL 栈 stack

头文件 **`#include <stack>`**  

`<stack>` 是 C++ 标准模板库（STL）的一部分，它实现了栈（stack）的数据结构。STL栈 的元素是线性排列的，但只允许在一端（栈顶）进行添加、移除和查看操作。

栈是一种==后进先出（LIFO，Last In First Out）的数据结构==。这种数据结构非常适合于需要"最后添加的元素最先被移除"的场景。

#### 常用方法

| 作用   | 用法              | 示例                    |
| ---- | --------------- | --------------------- |
| 构造   | `stack<类型> stk` | `stack<int> stk;`     |
| 进栈   | `.push(元素)`     | `stk.push(1);`        |
| 出栈   | `.pop()`        | `stk.pop();`          |
| 取栈顶  | `.top()`        | `int a = stk.top();`  |
| 判空   | `.empty()`      | 返回布尔值                 |
| 查看大小 | `.size()`       | `int a = stk.size();` |
以下是使用 `<stack>` 的基本语法：
```cpp
#include <iostream>
#include <stack>

using namespace std;

int main() {
	// 初始化一个STL栈
    stack<int> s;

    // 向栈中添加元素
    s.push(1);
    s.push(2);
    s.push(3);

    // 访问栈顶元素
    cout << "Top element is: " << s.top() << endl;

    // 移除栈顶元素
    s.pop();
    cout << "After popping, top element is: " << s.top() << endl;

    // 检查栈是否为空
    if (!s.empty()) {
        cout << "Stack is not empty." << endl;
    }

    // 打印栈的大小
    cout << "Size of stack: " << s.size() << endl;

    return 0;
}
```


#### 适用情形

如果不卡常的话，就可以直接用它而不需要手写栈了。  

另外，vector 也可以当栈用，vector 的 `.back()` 取尾部元素，就相当于取栈顶，`.push_back()` 相当于进栈，`.pop_back()` 相当于出栈。

#### 注意事项

STL的栈只允许访问栈顶，不可访问内部元素！ 
⚠️**下面都是错误用法**
```cpp
for (int i = 0; i < stk.size(); i++)
    cout << stk[i] << endl;
for (auto ele : stk)
    cout << stk << endl;
```

### STL队列 queue

头文件 **`#include <queue>`**  

C++ 标准库中的 `<queue>` 头文件提供了队列（queue）数据结构的实现。队列是一种先进先出（FIFO, First In First Out）的数据结构，它允许在一端添加元素（称为队尾），并在另一端移除元素（称为队首）。

#### 常用方法

| 作用       | 用法              | 示例                     |
| -------- | --------------- | ---------------------- |
| 构造       | `queue<类型> que` | `queue<int> que;`      |
| 进队       | `.push(元素)`     | `que.push(1);`         |
| 出队       | `.pop()`        | `que.pop();`           |
| 取队首      | `.front()`      | `int a = que.front();` |
| 取队尾      | `.back()`       | `int a = que.back();`  |
| 查看大小     | `.size()`       | `int a = que.size();`  |
| 检查队列是否为空 | `.empty()`      | 返回布尔值                  |

#### 适用情形

如果不卡常的话，就可以直接用它而不需要手写队列了。

#### 注意事项

和STL stack同样不可访问内部元素，只能访问队列头和尾的元素。
⚠️**下面都是错误用法**
```cpp
for (int i = 0; i < que.size(); i++)
    cout << que[i] << endl;
for (auto ele : que)
    cout << ele << endl;
```


## STL 算法库`<algorithm>`

C++ 标准库中的 `<algorithm>` 头文件提供了一组用于操作容器（如数组、向量、列表等）的算法。这些算法包括==排序、搜索、复制、比较==等，它们是编写高效、可重用代码的重要工具。

`<algorithm>` 头文件定义了一组模板函数，这些函数可以应用于任何类型的容器，只要容器支持迭代器。这些算法通常接受两个或更多的迭代器作为参数，表示操作的起始和结束位置。


### ✅ swap()

交换两个变量的值  
**用法示例**

```cpp
template< class T >
void swap( T& a, T& b );
```
```cpp
int a = 0, b = 1;
swap(a, b);
// now a = 1, b = 0

int arr[10] {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
swap(arr[4], arr[6]);
// now arr = {0, 1, 2, 3, 6, 5, 4, 7, 8, 9}
```

**注意事项**  
这个 swap 参数是==引用的==，不需要像 C 语言一样取地址。

### ✅ sort()

给容器中的元素排序  (使用快速排序)
**用法示例**
```cpp
void sort(container.begin(), container.end(), compare_function);
```

默认排序从小到大
```cpp
vector<int> arr{1, 9, 1, 9, 8, 1, 0};
sort(arr.begin(), arr.end());
// arr = [0, 1, 1, 1, 8, 9, 9]
```

如果要从大到小，则需要传比较器进去。
```cpp
vector<int> arr{1, 9, 1, 9, 8, 1, 0};
sort(arr.begin(), arr.end(), greater<int>());
// arr = [9, 9, 8, 1, 1, 1, 0]
```

如果需要完成特殊比较，则需要手写比较器。  
比较器函数返回值是 bool 类型，传参是需要比较的两个元素。记我们定义的该比较操作为 ：

- 若 ，则比较器函数应当返回 `true`
- 若 ，则比较器函数应当返回 `false`

**注意：** 如果 ，比较器函数必须返回 `false`

```cpp
bool cmp(pair<int, int> a, pair<int, int> b)
{
    if (a.second != b.second)
        return a.second < b.second;
    return a.first > b.first;
}

int main()
{
    vector<pair<int, int>> arr{{1, 9}, {2, 9}, {8, 1}, {0, 0}};
 sort(arr.begin(), arr.end(), cmp);
    // arr = [(0, 0), (8, 1), (2, 9), (1, 9)]
}
```

### ✅max() / min()

返回最大值 / 最小值的 **数值**  
**用法示例**

```cpp
int mx = max(1, 2); // 2
int mn = min(1, 2); // 1
```

使用列表构造语法传入一个列表，这样就能一次性给多个元素找最大值而不用套娃了：

```cpp

int mx = max({1, 2, 3, 4}); // 4
int mn = min({1, 2, 3, 4}); // 1
```

## C++ cmath库（数学函数）


`<cmath>` 是 C++ 标准库中的一个头文件，它提供了许多基本的数学运算和常数。这些数学函数可以执行基本的数学运算，如幂运算、三角函数、对数、绝对值等。

要使用 `<cmath>` 中的函数，你需要在你的 C++ 程序中包含这个头文件：
```cpp
#include <cmath>
```


### 常用数学函数
所有函数参数均支持 `int` / `long long` / `float` / `double` / `long double`

| 公式含义     | 示例                                                            | 英文全称        |
| -------- | ------------------------------------------------------------- | ----------- |
| 绝对值      | `abs(-1.0) == 1.0`<br>`abs(3.0) == 3.0`<br>`abs(-2.5) == 2.5` | absolute    |
| 幂次       | `pow(2, 3)== 2^3 == 8`                                        | power       |
| 平方根      | `sqrt(16) == √16 == 4`                                        | square root |
| 向上取整     | `ceil(2.1) == 3`                                              | ceiling     |
| 向下取整     | `floor(2.1) == 2`                                             | floor       |
| 舍入（四舍五入） | `round(2.1)==2`<br>`round(2.5)==3`                            | round       |




### lower\_bound() / upper\_bound()

在 **已升序排序** 的元素中，应用二分查找检索指定元素，返回对应元素迭代器位置。 **找不到则返回尾迭代器。**

- `lower_bound()`: 寻找 的第一个元素的位置
- `upper_bound()`: 寻找 的第一个元素的位置

怎么找 / 的第一个元素呢？

- 的第一个元素的前一个元素（如果有）便是 的第一个元素
- 的第一个元素的前一个元素（如果有）便是 的第一个元素

返回的是迭代器，如何转成下标索引呢？减去头迭代器即可。  
**用法示例**

```cpp
template< class ForwardIt, class T >
ForwardIt lower_bound( ForwardIt first, ForwardIt last, const T& value );
```
```cpp
vector<int> arr{0, 1, 1, 1, 8, 9, 9};
vector<int>::iterator it = lower_bound(arr.begin(), arr.end(), 7);
int idx = it - arr.begin();
// idx = 4
```

我们通常写成一行：

```cpp
vector<int> arr{0, 1, 1, 1, 8, 9, 9};
idx = lower_bound(arr.begin(), arr.end(), 7) - arr.begin(); // 4
idx = lower_bound(arr.begin(), arr.end(), 8) - arr.begin(); // 4
idx = upper_bound(arr.begin(), arr.end(), 7) - arr.begin(); // 4
idx = upper_bound(arr.begin(), arr.end(), 8) - arr.begin(); // 5
```

### reverse()

反转一个可迭代对象的元素顺序  
**用法示例**

```cpp
template< class BidirIt >
void reverse( BidirIt first, BidirIt last );
```
```cpp
vector<int> arr(10);
iota(arr.begin(), arr.end(), 1);
// 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
reverse(arr.begin(), arr.end());
// 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

### unique()

消除数组的重复 **相邻** 元素，数组长度不变，但是有效数据缩短，返回的是有效数据位置的结尾迭代器。  
例如：，下划线位置为返回的迭代器指向。

```cpp
template< class ForwardIt >
ForwardIt unique( ForwardIt first, ForwardIt last );
```

**用法示例**  
单独使用 unique 并不能达成去重效果，因为它只消除 **相邻** 的重复元素。但是如果序列有序，那么它就能去重了。  
但是它去重后，序列尾部会产生一些无效数据：，为了删掉这些无效数据，我们需要结合 erase.  
最终，给 vector 去重的写法便是：

```cpp
vector<int> arr{1, 2, 1, 4, 5, 4, 4};
sort(arr.begin(), arr.end());
arr.erase(unique(arr.begin(), arr.end()), arr.end());
```

### gcd() / lcm()

（C++17）返回最大公因数 / 最小公倍数

```cpp
int x = gcd(8, 12); // 4
int y = lcm(8, 12); // 24
```

如果不是 C++17，但是是 GNU 编译器（g++），那么可以用内置函数 `__gcd()`.  
当然， `gcd` / `lcm` 函数也挺好写，直接写也行（欧几里得算法）：

```cpp
int gcd(int a, int b)
{
    if (!b)
        return a;
    return gcd(b, a % b);
}
int lcm(int a, int b)
{
    return a / gcd(a, b) * b;
}
```

---

## 迭代器简介

### 迭代器是什么？

迭代器（iterator）类似于一个指针，专门拥有STL各种容器操作的指针。
对于一个`vector`，我们可以用索引（下标）遍历：

```cpp
for (int i = 0; i < a.size(); i++)
    cout << a[i] << endl;
```

我们同时也可以用迭代器来遍历：

```cpp
for (vector<int>::iterator it = a.begin(); it != a.end(); ++it)
    cout << *it << endl;
```
- `a.begin()` 是一个迭代器，指向的是第一个元素
- `a.end()` 是一个迭代器，指向的是最后一个元素 **再后面一位**
- 上述迭代器==具有自增运算符==，自增则迭代器向下一个元素移动
- ==迭代器与指针相似==，如果对它使用解引用运算符，即 `*it` ，就能取到对应值了

### 为何需要迭代器？

很多数据结构并不是线性的（例如红黑树），对于非线性数据结构，下标是无意义的。无法使用下标来遍历整个数据结构。  
迭代器的作用就是定义某个数据结构的遍历方式，通过迭代器的增减，代表遍历到的位置，通过迭代器便能成功遍历非线性结构了。  
例如，set 的实现是红黑树，我们是没法用下标来访问元素的。但是通过迭代器，我们就能遍历 set 中的元素了：

```cpp
for (set<int>::iterator it = st.begin(); it != st.end(); ++it)
    cout << *it << endl;
```

### 迭代器用法

对于 vector 容器，它的迭代器功能比较完整，以它举例：

- `.begin()` ：头迭代器
- `.end()` ：尾迭代器
- `.rbegin()` ：反向头迭代器
- `.rend()` ：反向尾迭代器
- 迭代器 `+` 整型：将迭代器向后移动
- 迭代器 `-` 整型：将迭代器向前移动
- 迭代器 `++` ：将迭代器向后移动 1 位
- 迭代器 `--` ：将迭代器向前移动 1 位
- 迭代器 `-` 迭代器：两个迭代器的距离
- `prev(it)` ：返回 it 的前一个迭代器
- `next(it)` ：返回 it 的后一个迭代器  
	对于其他容器，由于其结构特性，上面的功能不一定都有（例如 set 的迭代器是不能相减求距离的）

### 常见问题

**`.end()` 和 `.rend()` 指向的位置是无意义的值**  
对于一个长度为 10 的数组： `for (int i = 0; i < 10; i++)` ，第 10 位是不可访问的  
对于一个长度为 10 的容器： `for (auto it = a.begin(); it != a.end(); ++it)` ，.end 是不可访问的  
**不同容器的迭代器功能可能不一样**  
迭代器细化的话有正向、反向、双向，每个容器的迭代器支持的运算符也可能不同，因此不同容器的迭代器细节很有可能是不一样的。  
**删除操作时需要警惕**

为什么 3 没删掉？

```cpp
vector<int> a{1, 2, 3, 4};
for (auto it = a.begin(); it != a.end(); ++it)
    if (*it == 2 || *it == 3)
        a.erase(it);
// a = [1, 3, 4]
```

为啥 RE 了？

```cpp
vector<int> a{1, 2, 3, 4};
for (auto it = a.begin(); it != a.end(); ++it)
    if (*it == 4)
        a.erase(it);
```

**建议：如无必要，别用迭代器操作容器。（遍历与访问没关系）**

