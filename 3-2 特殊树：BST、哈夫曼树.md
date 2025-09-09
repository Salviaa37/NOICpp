---
tags:
  - clippings
  - Cpp
  - Coding
created:
---
难度系数：4

## BST（二叉搜索树）
二叉搜索树：在树中的任意一个节点，其左子树中的每个节点的值，都要小于这个节点的值，而右子树节点的值都大于这个节点的值。
![[Pasted image 20250626095045.png]]


### 构造一棵二叉搜索树
(1)二叉搜索树(BST)的定义
二叉排序树要么是空树，要么是满足以下性质的二叉树：
1)若它的左子树不空，则左子树上所有关键字的值均不大于(不小于)根的值。
2)若它的右子树不空，则右子树上所有关键字的值均不小于(不大于)根的值。
3)左右子树又各是一棵二叉排序树。

- 一般默认二叉排序树关键字按照==**左小右大**==分布，这样经过==中序遍历==，得到的关键字序列是==非递减有序==的。



### 二叉搜索树的表示与算法

#### 表示
BST的本质仍是一棵二叉树，二叉排序树的存储结构与一般的二叉树相同：
数据域存储关键字，指针域存储左子树和右子树的地址。
```cpp
typedef struct BTNode
{
	int key; //在查找算法中一般称节点存储的值为关键字（key）
	struct BTNode *lchild;
	struct BTNode *rchild;
	
}btnode;
```

#### 查找
显然，要查找的关键字，要么在根节点，要么在左子树，要么在右子树。

由二叉排序树的定义可以知道，根节点中的关键字将所有关键字分成了两部分，即大于根节点中关键字的部分和小于根节点中关键字的部分。
因此，我们可以将待查关键字先和根节点中的关键字比较，
  如果==相等==则查找成功；
	如果小于根节点则到左子树中去查找，无须考虑右子树中的关键字；
	如果大于根节点则到右子树中去查找，无须考虑左子树中的关键字。
如果来到当前树的子树根，则重复上述过程；
如果来到了节点的空指针域，则说明查找失败，这棵BST里面没有要查找的关键字。

查找可以使用递归或者迭代的方法。
查找代码（递归）
```cpp
//参数：指向二叉搜索树的指针、待查找的关键字target
//返回值：返回找到的节点指针
BTNode * n1;
BSTSearch(n1, 5); 

BTNode * BSTSearch(BTNode* bt, int target)
{
	if(bt == NULL)
		//空树或者节点指针域为空，返回空指针
		return NULL;
	else
	{
		if(bt->data == data)
			//等于当前节点关键字，查找成功 ，返回关键字的节点指针
			return bt;
		else if(bt->data < data)
			//小于当前节点关键字，向右子树查找
			return BSTSearch(bt->rchild, target);
		else
			//大于当前节点关键字，向左子树查找
			return BSTSearch(bt->lchild, target);
	}
}
```


与递归相比，使用while循环进行迭代的方法效率更高：
查找方法（迭代）
```cpp
BTNode* BSTSearch_Iter(BTNode* bt, int target)
{
	while(bt != nullptr)
	{
		//找到目标节点
		if(bt->data == target)
			return bt;
		//迭代下一层
		bt = bt->data >target? bt->lchild : bt->rchild;
	}
	// 查找失败，返回空指针
	return nullptr;
}

```


#### 增加 
二叉排序树是一个有顺序的查找表，插入一个关键字首先要找到插入位置。

对于一个不存在于二叉排序树中的关键字，其==查找不成功的位置==即为该关键字的插入位置。

如果要插入的关键字已经存在二叉树中，就不需要再插入了。

因此，只要对查找的算法稍微改造一下，在查找失败的位置添加新的节点，就实现了插入操作。
```cpp
//参数：指向二叉搜索树的指针、待插入的关键字target
//返回值：新的关键字是否插入成功，成功返回1，失败返回0

int BSTSearch(BTNode* &bt, int target) //注意，为了保持修改操作一致，这里需要引用指针
{
	
	if(bt == NULL)
	{
		//查到空指针域了，说明找到了插入位置，创建新节点插入
		BTNode * p = (BTNode*)malloc(sizeof(BTNode));
		p->lchild = p->rchild = NULL;
		p-> data = target;
		//插入成功，返回1
		return 1; 
	}
	//节点不空，继续查找
	else   
	{
		if(bt->data == target)
			//等于当前节点关键字，插入失败
			return 0;
		else if(bt->data < target)
			//小于当前节点关键字，向右子树查找
			return BSTSearch(bt->rchild, target);
		else
			//大于当前节点关键字，向左子树查找
			return BSTSearch(bt->lchild, target);
	}
}

```

#### 删除
当在二叉排序树中删除一个关键字时，不能把以该关键字所在的节点为根的子树都删除，而是只删除这一个节点，并保持二叉排序树的特性。
假设在二叉排序树上被删除节点为p，f为其亲节点，则删除节点p的过程分为以下3种情况：

1) p节点为叶子节点。由于删除叶子节点后不会破坏二叉排序树的特性，因此==直接删除==即可。
2) p节点只有一棵子树，此时只需将p删掉，然后将p的==子树直接连接到p的亲节点f上即可==；
   ![[Pasted image 20250626105118.png]]
3) p节点既有左子树又有右子树。此时可以将这种情况转化为1)或2)中的情况，做法为：
   ![[Pasted image 20250626105126.png]]
	1. 先找到右子树的最小值（或者左子树的最大值）；执行1）或者2）
	2. 先沿着p的右子树根节点的左指针一直往左走，直到来到右子树中最左边的一个节点l，l是右子树的最小值，也是p的中序遍历后继。
	3. 然后将p中的关键字用r中的关键字代替。最后判断，如果r是叶子节点，则按照1)中的方法删除r;如果r是非叶子节点，则按照2)中的方法删除r(此时的r不可能是有两个子树的节点)。

```cpp
//参数：指向二叉搜索树的指针、待删除的关键字target
//返回值：新的关键字是否插入成功，成功返回1，失败返回0
int BSTDelete(BTNode* &bt, int target)
{
    if (bt == NULL)
    {
        // 树为空或未找到目标节点，删除失败
        return 0;
    }
    else if (target < bt->data)
    {
        // 目标值小于当前节点值，向左子树查找
        return BSTDelete(bt->lchild, target);
    }
    else if (target > bt->data)
    {
        // 目标值大于当前节点值，向右子树查找
        return BSTDelete(bt->rchild, target);
    }
    else
    {
        // 找到目标节点
        if (bt->lchild == NULL && bt->rchild == NULL)
        {
            // 叶子节点，直接删除
            free(bt);
            bt = NULL;
        }
        else if (bt->lchild == NULL)
        {
            // 只有右子节点
            BTNode* temp = bt;
            bt = bt->rchild;
            free(temp);
        }
        else if (bt->rchild == NULL)
        {
            // 只有左子节点
            BTNode* temp = bt;
            bt = bt->lchild;
            free(temp);
        }
        else
        {
            // 有两个子节点，找到右子树的最小节点替换当前节点
            BTNode* temp = bt->rchild;
            while (temp->lchild != NULL)
            {
                temp = temp->lchild;
            }
            bt->data = temp->data;
            // 删除右子树中的最小节点
            return BSTDelete(bt->rchild, temp->data);
        }
        // 删除成功
        return 1;
    }
}

```


## 哈夫曼树 (Huffman Tree)

 [数据结构-构造哈夫曼树【详解+代码+图示】一文解惑！-阿里云开发者社区](https://developer.aliyun.com/article/1403653)
 [数据结构——哈夫曼树（Huffman Tree）](https://mp.weixin.qq.com/s/lNuzPMp-RSWcRkI2NYdFlw)
### 视频导入
[哈夫曼树和哈夫曼编码, 看完秒懂！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1qu411F7Zs/)

### 相关概念
我们在学习哈夫曼树之前需要先了解几个概念
首先需要说明几个关于路径的概念。

1)路径（Path）：路径是指从树中一个节点到另一个节点的分支所构成的路线。
![[Pasted image 20250626113318.png]]
2)路径长度（ Path Length）：路径长度是指路径上的分支数目。

3)权（Weight）：即节点存储的值，代表这个节点的权重。
3)带权路径长度（WPL，Weighted Path Length）：节点具有权值，从该节点到根之间的路径长度乘以节点的权值，就是该节点的带权路径长度。

4)树的带权路径长度(WPL of a tree):树的带权路径长度是指树中所有叶子节点的带权路径长度之和。

**什么是哈夫曼树?**

哈夫曼树 是一种最优树, 是一类带权路径长度最短的二叉树, 通过哈夫曼算法可以构建一棵哈夫曼树, 利用哈夫曼树可以构造一种不等长的二进制编码, 并且构造所得的哈夫曼编码是一种最优前缀码。


通俗来讲 : n 个带权节点均作为叶子节点, 构造出的一棵带权路径长度最短的二叉树, 则把这棵树称为 "哈夫曼树/赫夫曼树”（英语Huffman Tree） 或者 “最优二叉树” 。 本文均用 “哈夫曼树” 这一称谓。


第一次接触 哈夫曼树 的小伙伴可能对上述的定义不能第一时间消化, 我们可以先来明确几个学哈夫曼树之前必须掌握的几个会用到的名词的概念, 把这几个概念理解透了之后会帮助你更好的认识哈夫曼树.

### 构造哈夫曼树

#### 构造步骤
假设有n个权值，则构造出的哈夫曼树有n个叶子结点。n个权值分别设为 w1、w2、…、wn，则哈夫曼树的构造规则为：

(1) 将w1、w2、…，wn看成是有n 棵树的森林(每棵树仅有一个结点)；

(2) 在森林中选出两个根结点的权值最小的树合并，作为一棵新树的左、右子树，且新树的根结点权值为其左、右子树根结点权值之和；

(3)从森林中删除选取的两棵树，并将新树加入森林；

(4)重复(2)、(3)步，直到森林中只剩一棵树为止，该树即为所求得的哈夫曼树。



**注意:**

1.  最优二叉树的形态不唯一, 但是 WPL 最小.
2.  最优二叉树中, 权越大的叶子离根越近.

#### 图形化理解

例如: 已知权值 W = {2,5,9,6,7}, 请构造哈夫曼树.

![](https://ucc.alicdn.com/pic/developer-ecology/lhiq634h3jnbs_119ad4a0102c44b5a141c910f0358244.png?x-oss-process=image%2Fresize%2Cw_1400%2Cm_lfit%2Fformat%2Cwebp)
### 哈夫曼编码

### 证明结论

最优二叉树的形态不唯一, 但是 WPL 最小

*   先证明第一句话: **相同的权值构造出来的哈夫曼树形态不唯一**

同样是上述的例子, 我们可以构造出另一种形态的哈夫曼树, 同时我们可以计算出两种形态的二叉树的带权路径, 看看是否相等 ?

![](https://ucc.alicdn.com/pic/developer-ecology/lhiq634h3jnbs_af2a39ced5a14a4f97718c9795fc8646.png?x-oss-process=image%2Fresize%2Cw_1400%2Cm_lfit%2Fformat%2Cwebp)

*   然后我们来证明第二句话 : **证明哈夫曼算法构造出的哈夫曼树的 WPL 一定是最小的，且其他二叉树的 WPL 一定大于等于哈夫曼树的 WPL。**

**1. 定义：**

*   带权路径长度（WPL）是指每个叶子节点的权值乘以其深度的总和。

**2. 引理：**

*   对于任意的二叉树，交换任意两个叶子节点的位置，WPL 都会增加。

**3. 证明：**

*   **哈夫曼树的构造过程**：

1.  哈夫曼算法每次选择权值最小的两个节点合并，构造出一颗二叉树。
2.  重复这个过程，直到所有节点合并成一个根节点，形成哈夫曼树。

*   **交换叶子节点的影响**：

*   考虑哈夫曼树的构造过程，每次选择最小的两个节点合并，如果我们交换了两个叶子节点的位置，这两个节点的权值就不再是最小的了。
*   在构造过程中，我们总是选择权值最小的节点，因此交换叶子节点位置后，这两个节点的合并会在构造树的过程中晚一些，导致它们在更深的层次上，从而增加了 WPL。

*   如果这里不懂这里的影响是什么, 我会再下一篇的博客中详细解释, 这涉及到了 **贪心策略**  
    
*   **结论**：

*   由于哈夫曼算法保证了每次合并都选择最小权值的节点，任何对于叶子节点位置的交换都会导致合并发生在更深的层次上，增加 WPL。
*   因此，哈夫曼树的构造方式保证了 WPL 最小。

**4. 总结：**

*   哈夫曼树的构造方式保证了每次合并都是最优的选择，从而使得整个树的 WPL 最小。
*   如果使用其他方式构造二叉树，由于不能保证每次都选择最小权值的节点进行合并，可能会导致 WPL 更大。

### 代码实现

在构造哈夫曼树的过程中，我们首先需要定义一个节点类来表示树的节点，然后编写一个构造哈夫曼树的算法。

1.  定义了一个`HuffmanNode`类，用于表示哈夫曼树的节点
2.  在`HuffmanTree`类中，实现一个`buildHuffmanTree`方法，该方法接受一个权值数组
3.  使用优先队列来构造哈夫曼树
4.  通过`main`方法演示了如何使用这个方法来构造哈夫曼树

这个实现涉及到了优先队列（`PriorityQueue`），它是一个能够保证每次取出的元素都是队列中权值最小的元素的数据结构。在哈夫曼树的构建中，我们不断地合并权值最小的两个节点，而优先队列正好能够满足这个需求。

```
package src.test.java;
import java.util.PriorityQueue;
// 定义哈夫曼树节点类
class HuffmanNode implements Comparable<HuffmanNode> {
    int weight;          // 权值
    HuffmanNode left;    // 左子节点
    HuffmanNode right;   // 右子节点
    public HuffmanNode(int weight) {
        this.weight = weight;
    }
    // 实现Comparable接口，用于PriorityQueue的比较
    @Override
    public int compareTo(HuffmanNode other) {
        return this.weight - other.weight;
    }
}
public class HuffmanTree {
    // 构造哈夫曼树的方法
    public static HuffmanNode buildHuffmanTree(int[] weights) {
        // 使用优先队列来存储节点，每次都取出权值最小的两个节点进行合并
        PriorityQueue<HuffmanNode> pq = new PriorityQueue<>();
        
        // 将权值数组中的每个元素转化为节点并添加到优先队列中
        for (int weight : weights) {
            pq.add(new HuffmanNode(weight));
        }
        // 不断合并节点直到只剩一个节点，即哈夫曼树的根节点
        while (pq.size() > 1) {
            HuffmanNode left = pq.poll();   // 弹出权值最小的节点
            HuffmanNode right = pq.poll();  // 弹出权值次小的节点
            // 创建一个新节点，权值为两个子节点的权值之和
            HuffmanNode parent = new HuffmanNode(left.weight + right.weight);
            parent.left = left;
            parent.right = right;
            // 将新节点添加回优先队列
            pq.add(parent);
        }
        // 返回哈夫曼树的根节点
        return pq.poll();
    }
    // 打印哈夫曼树的方法（可选）
    public static void printHuffmanTree(HuffmanNode root, String prefix) {
        if (root != null) {
            System.out.println(prefix + root.weight);
            printHuffmanTree(root.left, prefix + "0");
            printHuffmanTree(root.right, prefix + "1");
        }
    }
    public static void main(String[] args) {
        int[] weights = {2, 5, 9, 6, 7};
        // 构造哈夫曼树
        HuffmanNode root = buildHuffmanTree(weights);
        // 打印哈夫曼树（可选）
        printHuffmanTree(root, "");
    }
}

```

### 测试
#### 代码
```
package src.test.java;
public class HuffmanTreeTest {
    public static void main(String[] args) {
        int[] weights = {2, 5, 9, 6, 7};
        // 构造哈夫曼树
        HuffmanNode root = HuffmanTree.buildHuffmanTree(weights);
        // 打印哈夫曼树的结构
        System.out.println("Huffman Tree Structure:");
        printHuffmanTreeStructure(root, "");
    }
    // 递归打印哈夫曼树的结构
    private static void printHuffmanTreeStructure(HuffmanNode root, String prefix) {
        if (root != null) {
            System.out.println(prefix + "Weight: " + root.weight);
            if (root.left != null || root.right != null) {
                System.out.println(prefix + "├── Left:");
                printHuffmanTreeStructure(root.left, prefix + "│    ");
                System.out.println(prefix + "└── Right:");
                printHuffmanTreeStructure(root.right, prefix + "     ");
            }
        }
    }
}

```

#### 结果对照

*   控制台输出结果

![](https://ucc.alicdn.com/pic/developer-ecology/lhiq634h3jnbs_b6a0819fe43b47f086971c767fe83818.png?x-oss-process=image%2Fresize%2Cw_1400%2Cm_lfit%2Fformat%2Cwebp)

*   哈夫曼树图形

![](https://ucc.alicdn.com/pic/developer-ecology/lhiq634h3jnbs_980d20b9b7dd4c1e8c7f107a6ec029ba.png?x-oss-process=image%2Fresize%2Cw_1400%2Cm_lfit%2Fformat%2Cwebp)

