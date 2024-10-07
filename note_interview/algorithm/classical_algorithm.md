# 经典算法

[TOC]

## 查找算法



### 二分查找

应用于有序的顺序表。

二分查找（binary search）是一种基于分治策略的高效搜索算法。它利用数据的有序性，每轮缩小一半搜索范围，直至找到目标元素或搜索区间为空为止。

```java
/* 二分查找（双闭区间） */
int binarySearch(int[] nums, int target) {
    // 初始化双闭区间 [0, n-1] ，即 i, j 分别指向数组首元素、尾元素
    int i = 0, j = nums.length - 1;
    // 循环，当搜索区间为空时跳出（当 i > j 时为空）
    while (i <= j) {
        int m = i + (j - i) / 2; // 计算中点索引 m
        if (nums[m] < target) // 此情况说明 target 在区间 [m+1, j] 中
            i = m + 1;
        else if (nums[m] > target) // 此情况说明 target 在区间 [i, m-1] 中
            j = m - 1;
        else // 找到目标元素，返回其索引
            return m;
    }
    // 未找到目标元素，返回 -1
    return -1;
}
```





## 二、排序算法

![image-20240918125720890](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202409181257057.png)

### 2.1 快速排序

快速排序：用数组的第一个数作为基准数据，然后将所有比它小的数都放到它左边，所有比它大的数都放到它右边，这个过程称为一趟快速排序。值得注意的是，快速排序不是一种稳定的排序算法，也就是说，多个相同的值的相对位置也许会在算法结束时产生变动。

1.2、算法过程

1. 数组为arr，设置两个变量left、right，排序开始的时候：left=0，right=arr.length-1
2. 以第一个数组元素作为关键数据，赋值给key，即key=A[left]
3. 从right开始向前搜索，即从后向前搜索(right–)，找到第一个小于key的值 arr[right]，将 arr[right] 和 arr[left] 的值交换，如果不满足（即3中arr[right]不小于key），则right=right-1
4. 从left开始向后搜索，即从前向后搜索(left++)，找到第一个大于key的 arr[left]，将arr[left] 和 arr[right] 的值交换，如果不满足（即4中arr[left]不大于key），则left=left+1
5. 重复第3、4步，直到left\==right (3,4步中，找到符合条件的值，进行交换的时候left， right指针位置不变。另外，left\==right 时循环结束）

```java
    private static void quickSort(int[] nums , int left , int right){
        if(left < right){
            int pivot = partition(nums,left,right);
            quickSort(nums,left,pivot - 1);
            quickSort(nums, pivot + 1, right);
        }
    }

    private static int partition(int[] nums, int left, int right) {
        int k = nums[left];
        while(left < right){
            while(left < right && nums[right] >= k) right--;
            nums[left] = nums[right];
            while(left < right && nums[left] <= k) left++;
            nums[right] = nums[left];
        }
        nums[left] = k;
        return left;
    }
```



### 2.2 堆排序

#### 实现堆的数据结构

### 2.3 归并排序

**时间复杂度：O(n log n)如何推导：**

了解归并排序的应该都知道，归并排序的时间复杂度是`O（nlogn）`，且这个时间复杂度是稳定的，不随需要排序的序列不同而产生波动。那这个时间复杂度是如何得来的呢？我们可以这样分析，假设我们需要对一个包含`n`个数的序列使用归并排序，并且使用的是递归的实现方式，那么过程如下：

- 递归的第一层，将`n`个数划分为`2`个子区间，每个子区间的数字个数为`n/2`；
- 递归的第二层，将`n`个数划分为`4`个子区间，每个子区间的数字个数为`n/4`；
- 递归的第三层，将`n`个数划分为`8`个子区间，每个子区间的数字个数为`n/8`;

  ......

- 递归的第`logn`层，将`n`个数划分为`n`个子区间，每个子区间的数字个数为`1`；

  我们知道，归并排序的过程中，需要对当前区间进行对半划分，直到区间的长度为`1`。也就是说，每一层的子区间，长度都是上一层的`1/2`。**这也就意味着，当划分到第logn层的时候，子区间的长度就是1了**。而归并排序的`merge`操作，则是从最底层开始（子区间为`1`的层），对相邻的两个子区间进行合并，过程如下：

- 在第`logn`层（最底层），每个子区间的长度为`1`，共`n`个子区间，每相邻两个子区间进行合并，总共合并`n/2`次。`n`个数字都会被遍历一次，所有这一层的总时间复杂度为`O（n）`；

  ......

- 在第二层，每个子区间长度为`n/4`，总共有`4`个子区间，每相邻两个子区间进行合并，总共合并`2`次。`n`个数字都会被遍历一次，所以这一层的总时间复杂度为`O（n）`；
- 在第一层，每个子区间长度为`n/2`，总共有`2`个子区间，只需要合并一次。`n`个数字都会被遍历一次，所以这一层的总时间复杂度为`O（n）`；

  通过上面的过程我们可以发现，对于每一层来说，在合并所有子区间的过程中，`n`个元素都会被操作一次，所以每一层的时间复杂度都是`O（n）`。而之前我们说过，归并排序划分子区间，将子区间划分为只剩`1`个元素，需要划分`logn`次。**每一层的时间复杂度为O（n），共有logn层，所以归并排序的时间复杂度就是O（nlogn）**。

#### 2.3.1 数组中的归并排序

- **时间复杂度：O(n log n)**
- **空间复杂度：O(n)**
  - 数组实现的归并排序通常需要一个额外的数组来存放中间结果，因此空间复杂度为 O(n)。这主要是因为在合并过程中需要分配与原数组大小相同的额外空间。

#### 2.3.2 链表中的归并排序

- **时间复杂度：O(n log n)**
- **空间复杂度：O(log n)**
  - 链表中的归并排序通常不需要额外的空间来存储数据，因为可以在原地操作链表。但是，由于递归调用的栈空间，空间复杂度是 O(log n)。这个空间复杂度是由于递归深度造成的，即递归栈所消耗的内存。

基于分治思想与快排相同

![image-20240903183344251](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202409031833340.png)

**数组的归并排序**

```java
	private static int[] aux;
    // 自顶向下的归并排序
    public static void mergeSort1(int[] a){
        aux = new int[a.length];
        mergeSort1(a,0,a.length-1);
    }
    public static void mergeSort1(int[] a, int lo, int hi){
        if(lo >= hi) return;
        // 相比于 (lo + hi)/2 可以防止溢出
        int mid = lo + (hi - lo)/2;
        mergeSort1(a,lo,mid);
        mergeSort1(a,mid,hi);
        merge(a,lo,mid,hi);
    }
    public static void merge(int[] a, int lo, int mid, int hi) {
        int i = lo ,j = mid + 1;
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k];
        }
        for (int k = lo; k <= hi; k++) {
            if(i > mid) a[k] = aux[j++];
            else if(j > hi) a[k] = aux[i++];
            else if(a[i] < a[j]) a[k] = aux[i++];
            else a[k] = aux[j++];
        }
    }


    // 自低向上的归并排序
    public static void mergeSort2(int[] a){
        int N = a.length;
        aux = new int[N];
        for (int sz = 1; sz < N; sz = sz + sz) {
            for (int lo = 0; lo < N - sz; lo = lo + sz + sz) {
                merge(a , lo, lo + sz -1 , Math.min(lo+sz+sz-1,N-1));
            }
        }

    }
```

**链表的归并排序**

```java
public ListNode sortList(ListNode head) {
    // 采用归并排序
    if (head == null || head.next == null) {
        return head;
    }
    // 获取中间结点
    ListNode mid = getMid(head);
    ListNode right = mid.next;
    mid.next = null;
    // 合并
    return merge(sortList(head), sortList(right));
}

private ListNode getMid(ListNode head) {
    // 快慢指针获取链表的中间节点
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

private ListNode merge(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0); // 虚拟头节点
    ListNode p = dummy;
    
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            p.next = l1;
            l1 = l1.next;
        } else {
            p.next = l2;
            l2 = l2.next;
        }
        p = p.next;
    }
    
    // 直接将剩余的链表连接到结果链表后面
    p.next = (l1 != null) ? l1 : l2;
    
    return dummy.next;
}

```

