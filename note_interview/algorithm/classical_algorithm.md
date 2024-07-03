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





## 排序算法

### 快速排序

### 堆排序

#### 实现堆的数据解构

### 归并排序

基于分治思想与快排相同

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

