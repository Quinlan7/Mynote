# leetcode刷题总结（基于Java）

*前言：我是按照代码随想录刷的，每日一道，打算先把所有种类的题刷一点，每个方法都学会一些，然后再去找难题刷*

## 算法技巧总结

### 通用技巧

1. 定义全局变量：在类中定义一个类属性，而不是定义在方法内，这样可以使很多题简单得多！！！
2. 当需要遍历（访问每个元素）时，并且会存在很多插入或者删除操作时，优先考虑==队列==，尤其是当遍历时有顺序要求时（比如按字典顺序），优先考虑==优先级队列==（会在插入删除时自动按顺序排序）。
3. 当查询模式匹配，给定 key 寻找 value，考虑使用map存储原始信息。
4. 当遇到复杂问题时：先对给定的数字排序总能简化问题
5. 当遇到需要查找一个集合中是否包含某个元素时，考虑使用Set<>结构存储原始数据
6. 原地问题：一般都是使用输入参数，来标记状态，从而达到 原地算法

### 双指针法

在处理数组和链表相关问题时，双指针技巧是经常用到的，双指针技巧主要分为两类：**左右指针**和**快慢指针**。

所谓左右指针，就是两个指针相向而行或者相背而行；而所谓快慢指针，就是两个指针同向而行，一快一慢。

对于单链表来说，大部分技巧都属于快慢指针（因为在链表中一般无法直接找到链表尾），它们都是通过一个 `fast` 快指针和一个 `slow` 慢指针配合完成任务。

在数组中并没有真正意义上的指针，但我们可以把索引当做数组中的指针，这样也可以在数组中施展双指针技巧。



这只是一个大概的介绍，还是要结合具体的题分析。

##### 前后指针

leetcode 15 ：三数之和

### KMP算法

关于KMP算法原理可以看这篇文章[（算法）通俗易懂的字符串匹配KMP算法及求next值算法](https://blog.csdn.net/qq_37969433/article/details/82947411)     
关于KMP算法的代码解析可以看我的文章[KMP算法](https://blog.csdn.net/miserable_world/article/details/130116323?spm=1001.2014.3001.5501)

##### kmp

leetcode 28 ：字符串模式匹配

### 回溯算法

+ **虽然回溯法很难，很不好理解，但是回溯法并不是什么高效的算法**。

  **因为回溯的本质是穷举，穷举所有可能，然后选出我们想要的答案**，如果想让回溯法高效一些，可以加一些剪枝的操作，但也改不了回溯法就是穷举的本质。

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```





### 递归调用

+ 最后需要返回的是 bool 类型

> 在某个条件返回时，返回当前判断 并(&&) 上递归调用
>
> 递归函数返回 bool

+ 最后需要返回的是 List

> 1. 如果是一层一个结果的话，可以直接List.addAll(递归调用)，递归函数返回list
> 2. 如果是递归到某个条件才能有一个结果的话（例如：二叉树的所有路径，只有找到叶子节点才能向list中增加一个结果），直接返回 void，在递归中判断条件向list中添加结果，并将中间参数作为递归函数的参数传递

+ 最后需要返回的是一个结点

> 可以在当前层，用当前层的结点的左右孩子指向，递归调用的结果。这样就可以每层返回自己对应的结点，在每层处理自己结点的逻辑，不用考虑它的父节点如何指向它的问题。

### 二叉搜索树

1. 二叉搜索树的中序遍历是一个递增序列

### 动态规划

> 做题的重点：
>
> 1. dp[]的含义
> 2. dp[]的递推关系
> 3. 当递推关系 很难 通过dp[i-k]的关系得出时，考虑更多的dp[]，记录更多数据。

动态规划：将复杂问题分解为==重叠子问题==，找出数学关系，列出==状态转移方程==，并且可以使用 ==DP table== 记录之前的状态，优化计算复杂度。

状态转移方程：dp 中对于状态转移方程的把握：要根据动作来

动态规划，为什么是一种比较优秀的算法，就是在于它可以把一些需要递归的问题，分解优化，为循环迭代计算的问题。

#### 背包问题总结

+ 01背包的核心代码

```java
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```

我们知道01背包内嵌的循环是从大到小遍历，为了保证每个物品仅被添加一次。

+ 完全背包的物品是可以添加多次的，所以要从小到大去遍历，即：

```java
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```

+ 如果组合问题需考虑元素之间的顺序，需将target放在外循环，将nums放在内循环。

```java
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```

#### 涉及到树的dp

+ 首先是对于 dp table 的存储，之前都是使用数组可以直接通过每个节点（下标）定位对应的值，而 树 结构，你无从获取 树 上某个节点的下标，所以我们使用 哈希表，即$Map<TreeNode,value>$
+ 对于状态的定义，常常会涉及到两个函数
  + 背包问题是某个物品是否可以放入背包，构成了函数的第二维
  + 树的问题是这个节点是否被选中，那么函数的第二维长度就是2，一种情况是当前节点入选，一种是当前节点不入选。这样也可以直接变为两个一维函数。
  + 当前节点被选/不被选，肯定会由题意带出更多的限制。由此列出状态转移方程
  + 一般的状态转移方程都是 $dp[i]$ 和 $dp[i-1],dp[i-2],nums[i]$ 的关系，而树形结构的话都是 $dp[i]$ 和 $dp[i.left],dp[i.right],i.value$ 的关系

#### 子序列相关dp

+ 解决两个字符串的动态规划问题，一般都是用两个指针 `i, j` 分别==指向两个字符串的最后==，然后一步步往前移动，缩小问题的规模。（也有可能指向字符串开始，优先考虑指向字符串最后）



### 图论

+ BFS算法的时间复杂度一定是优于DFS的，所有图论题首先考虑 BFS



### 子串（滑动窗口）

+ 子串问题 大多和 滑动窗口有关，一般都可以降低一个数量级的复杂度
+ 子串和
  + 子串和 可以利用 前缀和 来优化
+ 使用双端队列实现滑动窗口，实现在一次幂复杂度下寻找滑动窗口内的最大最小值



### 单调栈

单调栈（Monotonic Stack）是一种特殊的栈，具有以下特性：

- **单调递增栈**：栈内元素从栈底到栈顶单调递增。
- **单调递减栈**：栈内元素从栈底到栈顶单调递减。

单调栈是一种逻辑概念，实现的时候还是使用栈的结构去实现，用代码逻辑来控制是否单调。

#### 使用场景

单调栈主要用于在O(n)时间复杂度内解决与**数组中元素的相对位置和大小比较**相关的问题，常用于处理问题中寻找**下一个更大元素**或**下一个更小元素**的场景。

#### 例子

- **单调递增栈**：存储的元素从栈底到栈顶按值递增。如果遇到一个比栈顶元素大的元素，栈顶元素会被弹出。
- **单调递减栈**：存储的元素从栈底到栈顶按值递减。如果遇到一个比栈顶元素小的元素，栈顶元素会被弹出。

**单调栈的应用**:

1. **下一个更大元素问题**（Next Greater Element）
2. **每日温度问题**（Daily Temperatures）
3. **柱状图中最大的矩形**（Largest Rectangle in Histogram）





## 质量题目

[双指针技巧labuladong](https://labuladong.github.io/algo/di-ling-zh-bfe1b/shuang-zhi-fa4bd/)     

### 题目类型分组

##### 数组题目

[leetcode209](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/209.md)：滑动窗口;						√
[leetcode704](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/704.md)：二分查找;						√
[leetcode977](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/977.md)：头尾指针；     				√

##### 链表题目

[leetcode142](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Linked_Tables/142.md)：快慢指针；
[leetcode203](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Linked_Tables/203.md)：递归/迭代；     

##### 哈希表题目

[leetcode15](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/15.md)：头尾指针优化时间复杂度 $O(N^2)$ 变为 $O(N)$；      
[leetcode454](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/454.md)：简单的四数之和（分治）；      
[leetcode1](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/1.md)：哈希表或者头尾指针（先排序）；      

##### 字符串题目

[leetcode151](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/String/151.md)：反转字符串中的单词顺序，调包写的话很简单，但是也涉及到了正则表达式，不调包的实现难度比较大，可以仔细做做（先写个调包的算法，然后把调用的每个函数都实现一遍）；         
[leetcode28](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/String/28.md)：KMP算法，很有价值，尤其是对于next数组的求解；      

##### 栈与队列题目

[leetcode239](https://leetcode.cn/problems/sliding-window-maximum/)：双端队列，很不错，有一些小的点很难想到

leetcode347

##### 二叉树题目

leetcode101

leetcode102

leetcode110

leetcode257

leetcode617

leetcode501

leetcode236：这道题简直就是艺术，高质量

leetcode538

##### 回溯算法

leetcode93

leetcode332：高质量，不使用回溯算法

##### 贪心算法

leetcode53：动态规划了

leetcode134：有隐藏的数学关系，发现了时间复杂度就能降低一个数量级

leetcode135：和贪心真没什么关系，有一些小点很难发现

leetcode406：两个维度排序，排序后再处理

leetcode452：复杂问题先排序后处理

leetcode968：监控二叉树

##### 动态规划

leetcode70：分解为子问题，找到隐藏的状态转移方程

leetcode96： 二叉树性质的把握

leetcode416：经典背包问题

leetcode377：排列数

leetcode213：打家劫舍II

leetcode337：打家劫舍III，涉及到树的dp。有难度

leetcode123：买卖股票的最佳时机3，很复杂的dp，状态很多

leetcode718：子序列问题的 dp 定义

leetcode72：编辑距离（字符串的dp）

##### 图论

leetcode827：最大人工岛，难度刚刚好，代码量大，不错的题

leetcode127：单词接龙（BFS优于DFS）





### leetcode top 100

##### 双指针

leetcode11: 双指针

##### 子串

leetcode560：优化暴力解（前缀和 + 哈希表）

leetcode76：子串问题 使用 滑动窗口。 Java Integer 类型的比较！！

##### 普通数组

leetcode41：利用输入数组优化，空间复杂度

##### 链表

leetcode234：单链表需要从后向前遍历时，翻转链表降低复杂度

leetcode148：链表的自底向上的归并排序（时间复杂度O(n logn)，常数级空间复杂度）

leetcode146：LRU缓存

##### 树

leetcode437：树的前缀和

##### 图论

leetcode207：课程表（是否为拓扑排序）

##### 栈

leetcode155：最小栈，用O(1)的额外空间就可以实现最小栈

leetcode739：单调栈

leetcode84：单调栈的高级使用 hard

##### 堆

leetcode295：算法题中涉及到 **堆** 一般直接使用 **优先级队列** 即可

**动态规划**

leetcode152：无法通过dp[i-k]求出 dp[i]，定义两个 dp[] 数组

leetcode5：二维动态规划，也可以不用动态规划

**技巧**

leetcode136：异或操作

leetcode169：多数元素





### 蓝桥杯

附近最小：使用双端队列实现滑动窗口，实现在一次幂复杂度下寻找滑动窗口内的最大最小值

