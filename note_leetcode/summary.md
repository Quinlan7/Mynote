# leetcode刷题总结

*前言：我是按照代码随想录刷的，每日一道，打算先把所有种类的题刷一点，每个方法都学会一些，然后再去找难题刷*



### 双指针法

在处理数组和链表相关问题时，双指针技巧是经常用到的，双指针技巧主要分为两类：**左右指针**和**快慢指针**。

所谓左右指针，就是两个指针相向而行或者相背而行；而所谓快慢指针，就是两个指针同向而行，一快一慢。

对于单链表来说，大部分技巧都属于快慢指针（因为在链表中一般无法直接找到链表尾），它们都是通过一个 `fast` 快指针和一个 `slow` 慢指针配合完成任务。

在数组中并没有真正意义上的指针，但我们可以把索引当做数组中的指针，这样也可以在数组中施展双指针技巧。



这只是一个大概的介绍，还是要结合具体的题分析。

##### 前后指针

leetcode 15 ：三数之和





##### 参考

[双指针技巧labuladong](https://labuladong.github.io/algo/di-ling-zh-bfe1b/shuang-zhi-fa4bd/)     


### 数组题目（值得重复刷的）
[leetcode209](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/209.md)：滑动窗口     
[leetcode704](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/704.md)：二分查找      
[leetcode977](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Array/977.md)：头尾指针     

### 链表题目（值得重复刷的）
[leetcode142](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Linked_Tables/142.md)：快慢指针      
[leetcode203](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Linked_Tables/203.md)：递归/迭代     

### 哈希表题目（值得重复刷的）
[leetcode15](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/15.md)：头尾指针优化时间复杂度 $O(N^2)$ 变为 $O(N)$      
[leetcode454](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/454.md)：简单的四数之和（分治）      
[leetcode1](https://github.com/Quinlan7/Mynote/blob/main/note_leetcode/Hash_Tables/1.md)：哈希表或者头尾指针（先排序）      

