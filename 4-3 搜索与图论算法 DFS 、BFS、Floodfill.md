---
title: "深度优先搜索理论基础 | 代码随想录"
source: "https://programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E6%B7%B1%E6%90%9C%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E4%BB%A3%E7%A0%81%E6%A1%86%E6%9E%B6"
author:
published:
created: 2025-06-18
description: "代码随想录官网网站，代码随想录网站，代码随想录官网，代码随想录百度网盘，代码随想录知识星球，代码随想录八股文PDF，代码随想录刷题路线，代码随想录知识星球八股文"
tags:
  - "clippings"
---

## DFS的定义

**深度优先搜索算法（DFS，Depth First Search）**：是一种用于搜索/遍历树或图结构的算法。深度优先搜索算法采用了回溯思想，从起始节点开始，沿着一条路径尽可能深入地访问节点，直到无法继续前进时为止，然后回溯到上一个未访问的节点，继续深入搜索，直到完成整个搜索过程。

在深度优先搜索的过程中，我们需要将当前遍历节点 u 的相邻节点暂时存储起来，==以便于在回退的时候可以继续访问它们==。遍历到的节点顺序符合「后进先出」的特点，这正是「递归」和「堆栈」所遵循的规律，所以深度优先搜索可以通过「递归」或者「堆栈」来实现。


## 回溯
回溯算法（backtracking algorithm）是一种通过穷举来解决问题的方法，它的核心思想是从一个初始状态出发，暴力搜索所有可能的解决方案，当遇到正确的解则将其记录，直到找到解或者尝试了所有可能的选择都无法找到解为止。

回溯算法通常采用“深度优先搜索”来遍历解空间。在“二叉树”章节中，我们提到前序、中序和后序遍历都属于深度优先搜索。接下来，我们利用前序遍历构造一个回溯问题，逐步了解回溯算法的工作原理。
## DFS 与 BFS 

提到DFS，就不得不说和广度优先搜索（BFS，Board First Search）有什么区别。

- DFS是朝一个方向去搜，不到黄河不回头，直到遇到绝境了（图的终点或者树的叶子节点），搜不下去了，再换方向（换方向的过程就涉及到了==回溯==）。
- BFS是先把本节点所连接的所有节点遍历一遍，走到下一个节点的时候，再把连接节点的所有节点遍历一遍，搜索方向是四面八方的。



## DFS 搜索过程

前述DFS是朝一个方向搜，不到黄河不回头。 那么我们来举一个例子。==搜索所有路径==的例子

如图1，是一个==无向图==，我们要搜索从节点1到节点6的所有路径。
图1：
![图一](https://file1.kamacoder.com/i/algo/20220707093643.png)

那么DFS搜索的第一条路径是这样的：1->5->6（假设第一次沿默认方向，就找到了节点6），如图2：
![图二](https://file1.kamacoder.com/i/algo/20220707093807.png)


此时我们到达终点节点6，那么应该回头再去搜索其他方向了。 
撤销刚刚走过的路径2，来到节点5，节点5还有一条路径3。
我们改变方向，走路径3（红色线）到达节点4，再经过路径4，到达目的地。
![图三](https://file1.kamacoder.com/i/algo/20220707094011.png)
 这样又找到一条路线1->5->4->6。
 
在这里，撤销路径2，改为路径3，在DFS算法中其实就是回溯的过程（这一点很重要）。


执行回溯过程，撤销路径4，回到节点4。

这时，我们发现节点4还有一条路径，即路径5。经过路径5抵达节点3，节点3经过路径6抵达目的地，这样经过路径又找到了一条从节点1到节点6的路径，见下图。
![图四](https://file1.kamacoder.com/i/algo/20220707094322.png)
这样又找到了一条新路径:1->5->4->3->6.

再次回溯，路径6撤销回到节点3，改走路径7到达节点2。

现在的路径情况：1->5->4->3->2->?
节点2有两条路径可走，经过路径8，我们发现回到了出发点，这不是我们想要的路径。
另一条路径9， 又走回了节点4，两条路都是死路。
![图五](https://file1.kamacoder.com/i/algo/20220707094813.png)

现在，节点2所连接路径和节点3所链接的路径都走过了，只能回头撤销路径7和路径5，回到节点4，发现，节点4还可以经过路径10到达节点2 。 节点2经过路径11到达节点3，节点3如图：
![图六](https://file1.kamacoder.com/i/algo/20220707095232.png)

上图演示中，其实并没有把 所有的 从节点1 到节点6 的DFS（深度优先搜索）的过程都画出来，那样太冗余了，但已经把DFS关键的地方都涉及到了，总结为如下两点：

- 搜索方向：是认准一个方向搜，直到碰壁（到达终点）之后再换方向
- 换方向：是撤销原路径，改为该节点链接的下一个路径，回溯的过程。

## 代码框架

正是因为DFS搜索可一个方向，并需要回溯，所以用==递归的方式==来实现是最方便的。

有递归的地方就有回溯，那么回溯在哪里呢？



```cpp
void DFS(参数) {
    处理节点
    DFS(图，选择的节点); // 递归
    回溯，撤销处理结果
}
```

可以看到回溯操作就在递归函数的下面，递归和回溯是相辅相成的。

在讲解 [二叉树章节](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html) 的时候，==二叉树的递归法其实就是DFS==，而二叉树的迭代法，就是==BFS（广度优先搜索）==

所以 **DFS，BFS其实是基础搜索算法，也广泛应用与其他数据结构与算法中** 。

我们在回顾一下 [回溯法](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html) 的代码框架：

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }
    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

回溯算法，其实就是DFS的过程，这里给出DFS的代码框架：

```cpp
void DFS(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本节点所连接的其他节点) {
        处理节点;
        DFS(图，选择的节点); // 递归
        回溯，撤销处理结果
    }
}
```

可以发现DFS的代码框架和回溯算法的代码框架是差不多的。

下面解读DFS的代码框架。

## 深搜三部曲

在 [二叉树递归讲解](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html) 中，给出了递归三部曲。

[回溯算法](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html) 讲解中，给出了 回溯三部曲。

其实深搜也是一样的，深搜三部曲如下：

1. 确认递归函数，参数
```cpp
void DFS(参数)
```

通常我们递归的时候，我们递归搜索需要了解哪些参数，其实也可以在写递归函数的时候，发现需要什么参数，再去补充就可以。

一般情况，深搜需要二维数组结构保存所有路径，需要一维数组保存单一路径，这种保存结果的数组，我们可以定义一个全局变量，避免让我们的函数参数过多。

例如这样：

```cpp
vector<vector<int>> result; // 保存符合条件的所有路径
vector<int> path; // 起点到终点的路径
void DFS (图，目前搜索的节点)
```

但这种写法看个人习惯，不强求。

2. 确认终止条件

终止条件很重要，很多同学写DFS的时候，之所以容易死循环，栈溢出等等这些问题，都是因为终止条件没有想清楚。

```cpp
if (终止条件) {
    存放结果;
    return;
}
```

终止条件不仅是结束本层递归，同时也是我们收获结果的时候。

另外，其实很多DFS写法，没有写终止条件，其实终止条件写在了， 隐藏在下面DFS递归的逻辑里了，也就是不符合条件，直接不会向下递归。这里如果大家不理解的话，没关系，后面会有具体题目来讲解。

3. 处理目前搜索节点出发的路径

一般这里就是一个for循环的操作，去遍历 目前搜索节点 所能到的所有节点。

```cpp
for (选择：本节点所连接的其他节点) {
    处理节点;
    DFS(图，选择的节点); // 递归
    回溯，撤销处理结果
}
```

不少录友疑惑的地方，都是 DFS代码框架中for循环里分明已经处理节点了，那么 DFS函数下面 为什么还要撤销的呢。

如图七所示， 路径2 已经走到了 目的地节点6，那么 路径2 是如何撤销，然后改为 路径3呢？ 其实这就是 回溯的过程，撤销路径2，走换下一个方向。

![图七](https://file1.kamacoder.com/i/algo/20220708093544.png)



总结一下，深度搜索三步：

1. 确认递归函数，参数
2. 确认终止条件
3. 处理目前搜索节点出发的路径

## DFS 代码

深度优先搜索可以用于寻路径、寻找特定节点等。

### 寻找特定节点

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

// 邻接表表示图
unordered_map<int, vector<int>> graph;
// 标记节点是否已访问，防止环路和重复访问
vector<bool> visited;

// DFS函数，搜索图中是否存在目标节点
bool dfs(int current, int target) {
    // 如果当前节点就是目标节点，返回true
    if (current == target) {
        return true;
    }
    
    // 标记当前节点为已访问
    visited[current] = true;
    
    // 递归搜索相邻节点
    for (int next : graph[current]) {
        // 如果相邻节点未被访问
        if (!visited[next]) {
            // 如果从相邻节点出发能找到目标节点，返回true
            if (dfs(next, target)) {
                return true;
            }
        }
    }
    
    // 从当前节点出发无法找到目标节点
    return false;
}

// 检查图中是否存在从start出发能到达的目标节点target
bool existsPath(int start, int target, int n) {
    // 初始化visited数组
    visited.resize(n + 1, false);
    
    // 开始DFS搜索
    return dfs(start, target);
}

int main() {
    int n = 6; // 节点数量
    int start = 1; // 起始节点
    int target = 6; // 目标节点
    
    // 构建图示例中的图
    graph[1] = {2, 3, 4};
    graph[2] = {1, 3, 5};
    graph[3] = {1, 2, 5};
    graph[4] = {1, 5};
    graph[5] = {2, 3, 4, 6};
    graph[6] = {5};
    
    // 检查是否存在从start到target的路径
    if (existsPath(start, target, n)) {
        cout << "存在从节点" << start << "到节点" << target << "的路径！" << endl;
    } else {
        cout << "不存在从节点" << start << "到节点" << target << "的路径！" << endl;
    }
    
    return 0;
}
```

### 图遍历

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

// 邻接表存储图
unordered_map<int,vector<int>> graph;
// 标记节点是否访问
vector<bool> visited;
// 存储遍历顺序
vector<int> traversal;

// 一个参数，当前节点
// dfs 遍历当前节点以及所有可达节点
void dfs(int current){
	if(visit[current]){
		return;
	}
	// 标记当前节点为已经访问
	visited[current] = true;

	// 处理当前节点
	traversal.push_back(current);
	cout << "访问：" << current << endl;
	
	// 递归地访问所有未访问的节点
	for(int next : graph[current]){
		if(!visited[next]){
			dfs(next);
		}
	}
}

void dfs(int u) {
    if(visit[u]) {                                // 1
        return ;
    }
    visit[u] = true;                              // 2
    dfs_add(u);                                   // 3
    for(int i = 0; i < MAXN; ++i) {
        int v = i;
        if(adj[u][v]) {                           // 4
            dfs(v);                               // 5
        }
    }
}

```

### 所有路径

下面是一个寻找图中从起点到终点所有路径的DFS实现示例：

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

// 邻接表表示图
unordered_map<int, vector<int>> graph;
// 保存所有符合条件的路径
vector<vector<int>> result;
// 保存当前路径
vector<int> path;
// 标记节点是否已访问，防止环路
vector<bool> visited;

// DFS函数，搜索从当前节点到目标节点的所有路径
void dfs(int current, int target) {
    // 1. 处理当前节点
    path.push_back(current);
    visited[current] = true;
    
    // 2. 终止条件：找到目标节点
    if (current == target) {
        result.push_back(path); // 找到一条路径，保存结果
    } else {
        // 3. 递归搜索相邻节点
        for (int next : graph[current]) {
            if (!visited[next]) {
                dfs(next, target);
            }
        }
    }
    
    // 4. 回溯：恢复状态，以便探索其他路径
    visited[current] = false;
    path.pop_back();
}

// 找出从start到end的所有路径
vector<vector<int>> findAllPaths(int start, int end, int n) {
    result.clear();
    path.clear();
    visited.resize(n + 1, false);
    
    dfs(start, end);
    return result;
}

int main() {
    // 构建图示例中的图 (基于文档中的例子)
    // 节点编号为1到6
    graph[1] = {2, 3, 4};
    graph[2] = {1, 3, 5};
    graph[3] = {1, 2, 5};
    graph[4] = {1, 5};
    graph[5] = {2, 3, 4, 6};
    graph[6] = {5};
    
    // 寻找从节点1到节点6的所有路径
    vector<vector<int>> paths = findAllPaths(1, 6, 6);
    
    // 打印所有路径
    cout << "从节点1到节点6的所有路径：" << endl;
    for (const auto& path : paths) {
        for (int node : path) {
            cout << node << " -> ";
        }
        cout << "完成" << endl;
    }
    
    return 0;
}
```

这个例子使用了邻接表表示图，并实现了DFS来搜索从起点到终点的所有路径。代码展示了深搜三部曲的实现：
1. 定义递归函数及其参数
2. 设置终止条件（找到目标节点）
3. 处理当前节点的所有出路（通过遍历所有相邻节点）

注意回溯的操作：搜索完一条路径后，需要撤销当前节点的访问状态，并从路径中移除，以便探索其他可能的路径。


## Floodfill算法


题频：1
难度系数：5
2022CSP-J 第一轮 17 

视频导入
[有心了！2022CSP-J初赛 完善程序 T.2 洪水填充动画演示_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1X3tLeZEzg/?vd_source=574e0a38261401341cd391f17b73fdad)

漫水填充算法，顾名思义就像洪水漫过一样，把一块连通的区域填满，当然水要能漫过需要满足一定的条件，可以理解为满足条件的地方就是低洼的地方，水才能流过去。在图像处理中就是给定一个种子点作为起始点，向附近相邻的像素点扩散，把颜色相同或者相近的所有点都找出来，并填充上新的颜色，这些点形成一个连通的区域。 漫水填充算法可以用来标记或者分离图像的一部分，可实现类似Windows 画图油漆桶功能，或者PS里面的魔棒选择功能。

算法实现

漫水填充算法实现最常见有四邻域像素填充法，八邻域 像素填充法，基于扫描线的填充方法。根据代码实现方式又可以分为递归与非递归。

[图像处理之漫水填充算法（flood fill algorithm）-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1013350)


例题【2022CSP-J 第一轮 17】
#### （洪水填充）

现有用字符标记像素颜色的 8×8 图像。颜色填充的操作描述如下：给定起始像素的位置待填充的颜色，将起始像素和所有可达的像素（可达的定义：经过一次或多次的向上、下、左、右四个方向移动所能到达且终点和路径上所有像素的颜色都与起始像素颜色相同），替换为给定的颜色。

试补全程序。

```cpp
#include<bits/stdc++.h>
using namespace std;

const int ROWS = 8;
const int COLS = 8;

struct Point {
    int r, c;
    Point(int r, int c): r(r), c(c) {}
};

bool is_valid(char image[ROWS][COLS], Point pt,
              int prev_color, int new_color) {
    int r = pt.r;
    int c = pt.c;
    return (0 <= r && r < ROWS && 0 <= c && c < COLS &&
            ① && image[r][c] != new_color);
}

void flood_fill(char image[ROWS][COLS], Point cur, int new_color) {
    queue<Point> queue;
    queue.push(cur);

    int prev_color = image[cur.r][cur.c];
    ②;

    while (!queue.empty()) {
        Point pt = queue.front ();
        queue.pop ();

        Point points[4] = {③, Point(pt.r - 1, pt.c),
                           Point(pt.r, pt.c + 1), Point(pt.r, pt.c - 1)};
        for (auto p : points) {
            if (is_valid(image, p, prev_color, new_color)) {
                ④;
                ⑤;
            }
        }
    }
}

int main() {
    char image[ROWS][COLS] = {{'g', 'g', 'g', 'g', 'g', 'g', 'g', 'g'},
                              {'g', 'g', 'g', 'g', 'g', 'g', 'r', 'r'},
                              {'g', 'r', 'r', 'g', 'g', 'r', 'g', 'g'},
                              {'g', 'b', 'b', 'b', 'b', 'r', 'g', 'r'},
                              {'g', 'g', 'g', 'b', 'b', 'r', 'g', 'r'},
                              {'g', 'g', 'g', 'b', 'b', 'b', 'b', 'r'},
                              {'g', 'g', 'g', 'g', 'g', 'b', 'g', 'g'},
                              {'g', 'g', 'g', 'g', 'g', 'b', 'b', 'g'}};

    Point cur(4, 4);
    char new_color = 'y';

    flood_fill(image, cur, new_color);

    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            cout << image[r][c] << '';
        }
        cout << endl;
    }
//输出:
// g g g g g g g g
// g g g g g g r r
// g r r g g r g g
// g y y y y r g r
// g g g y y r g r
// g g g y y y y r
// g g g g g y g g
// g g g g g y y g

    return 0;
}
```

①~⑤处应填（ ）

1
 A. `image[r][c] == prev_color` 
B. `image[r][c] != prev_color` 
C. `image[r][c] == new_color` 
D. `image[r][c] != new_color`

2
 A. `image[cur.r+1][cur.c] = new_color` 
 B. `image[cur.r][cur.c] = new_color` 
 C. `image[cur.r][cur.c+1] = new_color`
 D. `image[cur.r][cur.c] = prev_color`


3.
 A. `Point(pt.r, pt.c)` 
 B. `Point(pt.r, pt.c+1)` 
 C. `Point(pt.r+1, pt.c)` 
 D. `Point(pt.r+1, pt.c+1)`

4.
 A.`prev_color = image[p.r][p.c]` 
 B. `new_color = image[p.r][p.c]` 
 C. `image[p.r][p.c] = prev_color` 
 D. `image[p.r][p.c] = new_color`

5.
A. `queue.push(p)` 
B. `queue. push (pt)` 
C. `queue.push(cur)` 
D. `queue. push(Point (ROWS, COLS))`


### 应用
Floodfill算法主要用于图像处理、游戏开发中。

Windows中的画图工具中的填充功能，其实就是使用`洪水填充算法`来实现的。下面的动图演示了填充过程。Photoshop中的魔棒选区功能也使用了Floodfill算法。
[03836651838d2496686b9c58d648f0fa.gif (1475×1012)](https://i-blog.csdnimg.cn/blog_migrate/03836651838d2496686b9c58d648f0fa.gif#pic_center)

游戏地图
在计算机游戏中，地图探索算法使用洪水填充来寻找从给定位置可到达的所有连通区域，如迷宫游戏或地图探索类游戏。在生成随机地图时，应该避免生成`角色`无法到达的`死路`。可以通过`洪水填充算法`来实现，比如下面这个游戏demo其中地图生成中就使用到了。
[fab50a6915863d42ca7035cad73ff5f7.gif (1024×771)](https://i-blog.csdnimg.cn/blog_migrate/fab50a6915863d42ca7035cad73ff5f7.gif#pic_center)



本题共 **15** 分