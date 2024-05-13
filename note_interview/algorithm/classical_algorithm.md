# 经典算法

[TOC]

## 排序算法

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

