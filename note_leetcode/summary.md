# leetcode刷题总结（基于Java）

*前言：我是按照代码随想录刷的，每日一道，打算先把所有种类的题刷一点，每个方法都学会一些，然后再去找难题刷*

### 通用技巧

1. 定义全局变量：在类中定义一个类属性，而不是定义在方法内，这样可以使很多题简单得多！！！

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





### 参考

[双指针技巧labuladong](https://labuladong.github.io/algo/di-ling-zh-bfe1b/shuang-zhi-fa4bd/)     

##### 数组题目（值得重复刷的）

[leetcode209](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/209.md)：滑动窗口；     
[leetcode704](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/704.md)：二分查找；      
[leetcode977](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/977.md)：头尾指针；     

##### 链表题目（值得重复刷的）

[leetcode142](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Linked_Tables/142.md)：快慢指针；      
[leetcode203](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Linked_Tables/203.md)：递归/迭代；     

##### 哈希表题目（值得重复刷的）

[leetcode15](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/15.md)：头尾指针优化时间复杂度 $O(N^2)$ 变为 $O(N)$；      
[leetcode454](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/454.md)：简单的四数之和（分治）；      
[leetcode1](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/1.md)：哈希表或者头尾指针（先排序）；      

##### 字符串题目（值得重复刷的）

[leetcode151](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/String/151.md)：反转字符串中的单词顺序，调包写的话很简单，但是也涉及到了正则表达式，不调包的实现难度比较大，可以仔细做做（先写个调包的算法，然后把调用的每个函数都实现一遍）；         
[leetcode28](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/String/28.md)：KMP算法，很有价值，尤其是对于next数组的求解；      

##### 栈与队列题目（值得重复刷的）

[leetcode239](https://leetcode.cn/problems/sliding-window-maximum/)：双端队列，很不错，有一些小的点很难想到

leetcode347

##### 二叉树题目（值得重复刷的）

leetcode101

leetcode102

leetcode110

leetcode257

leetcode617

leetcode501

leetcode236：这道题简直就是艺术，高质量

leetcode538

##### 回溯算法（值得重复刷的）
