# 笔试算法

[TOC]

## 一、技巧

### 1.注意数据的表示范围







### 2.输入输出请使用BufferedReader和BufferedWriter

在很多面试题目中，算法超时的原因并不是算法本身的时间复杂度，而可能是由于 常规的 `Scanner ` 和 `System.out.println` 的输入输出方式造成的性能瓶颈。在题目的很多测试用例中总是会有输入和输出很多的用例，所以我们都是用 `bufferedReader` 和 `BufferedWriter` 来进行输入和输出。

#### API

**bufferedReader**

+ `String readLine()`: 读取一行文本（不包括行终止符），如果已经到达流的末尾，则返回 `null`。
+ `int read()`: 读取单个字符，返回字符的整数值，如果已到达流的末尾，返回 -1。
+ `void close()`: 关闭流并释放相关资源。

**BufferedWriter**

+ `void write(String s)`: 写入一个字符串。

+ `void newLine()`: 写入一个行终止符（系统相关）。

+ `void flush()`: 刷新缓冲区，将所有缓冲的字符实际写入到目标流。

+ `void close()`: 关闭流并释放相关资源。

#### 示例

首先导包

```java
import java.io.*;
```

**bufferedReader**

注意要抛出异常！

```java
public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        String[] firstLine = reader.readLine().split(" ");
        int n = Integer.parseInt(firstLine[0]);
        int m = Integer.parseInt(firstLine[1]);
        int q = Integer.parseInt(firstLine[2]);
		
    	reader.close();
		
    }
```

**BufferedWriter**

可以在字符串里面使用  \n  换行。

```java
public static void main(String[] args) throws IOException {
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(System.out));
        
    	List<Boolean> list = new ArrayList<>();
        for (int i = list.size() - 1; i >= 0; i--) {
            writer.write(list.get(i) ? "Yes\n" : "No\n");
        }
        writer.flush();
		writer.close();
    }
```



### 3.并查集

#### 3.1 解决的问题

动态连通性问题。即可以以极低的复杂度解决图中两个节点是否连通的问题，并且可以动态的加入连通的边。但是**不可以删除边，并且无法输出路径，只是快速获取连通的关系。**

#### 3.2 并查集定义

**并查集（Union Find）**：一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。不交集指的是一系列没有重复元素的集合。

并查集主要支持两种操作：

- **合并（Union）**：将两个集合合并成一个集合。
- **查找（Find）**：确定某个元素属于哪个集合。通常是返回集合内的一个「代表元素」。

#### 3.3 两种实现思路

+ 用数组实现，一般使用 **快速合并**
  + 快速查询：parent[i] 直接存储 i 的根节点
  + 快速合并：parent[i] 存储 i 的父节点
+ 用Map实现：一般不使用 Map 实现的并查集，但是在某些题目中，使用数组实现的并查集会 OOM ，因为关系很少，但是节点很多使用数组很浪费空间，所以可以使用 Map 的实现。

#### 3.4 优化：路径压缩

在集合很大或者树很不平衡时，使用上述「快速合并」思路实现并查集的代码效率很差，最坏情况下，树会退化成一条链，单次查询的时间复杂度高达 *O*(*n*)。并查集的最坏情况如下图所示。

![并查集最坏情况](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408191548527.png)

**路径压缩（Path Compression）**：在从底向上查找根节点过程中，如果此时访问的节点不是根节点，则我们可以把这个节点尽量向上移动一下，从而减少树的层树。这个过程就叫做路径压缩。

+ 隔代压缩：在查询时，两步一压缩，一直循环执行「把当前节点指向它的父亲节点的父亲节点」这样的操作，从而减小树的深度。

![路径压缩：隔代压缩](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408191545916.png)

```java
private static int find(x){                      
    while(parent[x] != x){
        parent[x] = parent[parent[x]];      
        x = parent[x];
    }                      
    return x;
}
```

+ 完全压缩：在查询时，把被查询的节点到根节点的路径上的所有节点的父节点设置为根节点，从而减小树的深度。也就是说，在向上查询的同时，把在路径上的每个节点都直接连接到根上，以后查询时就能直接查询到根节点。

![路径压缩：完全压缩](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408191554115.png)

```java
private static int find(x){                      
    if(parent[x] != x){
        parent[x] = find(parent[x]);      
    }                      
    return x;
}
```

> 可以看到完全路径压缩需要递归，性能未必比隔代压缩好，隔代压缩性能稳定，默认使用隔代压缩！



#### 3.5 优化：按秩合并

> 实际上的应用效果一般，运行时间没有什么提升

**按秩合并（Union By Rank）**：指的是在每次合并操作时，都把「秩」较小的树根节点指向「秩」较大的树根节点。

这里的「秩」有两种定义，一种定义指的是树的深度；另一种定义指的是树的大小

+ 按深度合并

```java
private static void union(x, y){                        
        root_x = find(x)
        root_y = find(y)
        if(root_x == root_y){
            return;
        }                        
        if(rank[root_x] < rank[root_y]){
            parent[root_x] = root_y;
        } else if(rank[root_y] > rank[root_y]){
            parent[root_y] = root_x;   
        }else{
            parent[root_x] = root_y                
            rank[root_y] += 1 
        }              
        return;
}
```

> 为什么在路径压缩的过程中不用更新 rank 值或者 size 值呢？
>
> 其实，代码中的 rank 值或者 size* 值并不完全是树中真实的深度或者集合元素个数。
>
> 这是因为当我们在代码中引入路径压缩之后，维护真实的深度或者集合元素个数就会变得比较难。此时我们使用的 rank 值或者 size 值更像是用于当前节点排名的一个标志数字，只在合并操作的过程中，用于比较两棵树的权值大小。

#### 3.6 代码实现

+ 数组快速合并+隔代压缩（**一般使用这个就足够了**）

```java
public class UnionFind {
    private int[] parent;
 
    public UnionFind(int size) {
        parent = new int[size];
        for (int i = 0; i < size; i++) {
            parent[i] = i;
        }
    }
 
    public int find(int x) {
        while (x != parent[x]) {
            parent[x] = parent[parent[x]];      
        	x = parent[x];
        }
        return x;
    }
 
    public void union(int x, int y) {
        int RootX = find(x);
        int RootY = find(y);
        if (RootX == RootY) {
            return;
        }
        parent[RootX] = RootY;
    }
}
```

+ 如果使用数组会OOM，再考虑使用 Map 的实现

```java
static class UnionFind {
        Map<Integer, Integer> repre;

        public UnionFind(int n) {
            repre = new HashMap<>();
        }

        public int find(int x) {
            int xParent = repre.getOrDefault(x, x);
            if (xParent != x) {
                repre.put(x, find(xParent));
            }
            return repre.getOrDefault(x, x);
        }

        public void union(int x, int y) {
            int rootX = find(x), rootY = find(y);
            if (rootX == rootY) return;
            repre.put(rootX, rootY);
        }
}
```







## 二、题目

### 2.1 美团

#### 2024 春招第一场笔试

+ 小美的朋友关系

