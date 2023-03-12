# ArratList集合遍历删除元素

*前言：功能需要在一个ArrayList中删除掉不符合条件的元素一开始的想法是用for循环，其中判断条件，不符合直接List.remove(index)，但是debug时直接在for循环的条件处爆了索引不存在，想到了是因为remove元素后，ArrayList的size就减一了并且删除一个元素后，后面所有的元素索引都会前移，这样for循环的结束条件ArrayList.size()就会一直变化，导致的结果就是只要删除了元素就一定无法遍历ArrayList中的所有元素（具体说就是删除一个元素就会少遍历一个元素），所以再对ArrayList进行遍历删除元素时序言一个好方法。*



##### 序言：（==需要增删的时候最好直接用LinkedList==）

在java集合中，list列表应该是我们最常使用的，它有两种常见的实现类：ArrayList和LinkedList。ArrayList底层是数组，查找比较方便；LinkedList底层是链表，更适合做新增和删除。但实际开发中，我们也会遇到使用ArrayList需要删除列表元素的时候。虽然ArrayList类已经提供了remove方法，不过其中有潜在的坑，下面将介绍remove方法的几种错误用法以及几种正确用法。

### 一. 错误用法

##### for循环中使用remove(int index)，列表从前往后遍历

ArrayList.remove(int index)源码的执行逻辑是：移除列表指定位置的一个元素，将该元素后面的元素们往左移动一位。返回被移除的元素。如果在for循环中调用了多次ArrayList.remove()，那代码**执行结果是不准确**的，因为每次每次调用remove函数，ArrayList列表都会改变数组长度，被移除元素后面的元素位置都会发生变化。

##### 直接使用list.remove(Object o)

ArrayList.remove(Object o)源码的逻辑和ArrayList.remove(int index)大致相同：列表索引坐标从小到大循环遍历，若列表中存在与入参对象相等的元素，则把该元素移除，后面的元素都往左移动一位，返回true，若不存在与入参相等的元素，返回false。

并且这个方法会涉及到List的可以存储多个相同值的特性，如果有多个相等的元素，他只会删除一个。



### 二. 正确方法

##### 在for循环之后使用removeAll(Collection<?> c)

种方法思路是for循环内使用一个集合存放所有满足移除条件的元素，for循环结束后直接使用removeAll方法进行移除。

```java
List<Long> removeList = new ArrayList<>();
        for (int i = 0; i < list.size(); i++) {
            if (i % 2 == 0) {
                removeList.add(list.get(i));
            }
        }
        list.removeAll(removeList);
```



##### list转为迭代器Iterator的方式

迭代器就是一个链表，直接使用remove操作不会出现问题。

```java
Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
	if (it.next() % 2 == 0)
		it.remove();
}
```



##### for循环中使用remove(int index), 列表从后往前遍历

也是for循环，为啥从后往前遍历就是正确的呢。因为每次调用remove(int index)，index后面的元素会往前移动，如果是从后往前遍历，index后面的元素发生移动，跟index前面的元素无关，我们循环只去和前面的元素做判断，因此就没有影响。

```java'
for (int i = list.size() - 1; i >= 0; i--) {
            if (list.get(i).longValue() == 2) {
                list.remove(i);
            }
        }
```



##### 使用while循环

使用while循环，删除了元素，索引便不+1，在没删除元素时索引+1

```java
int i=0;
while (i<list.size()) {
	if (i % 2 == 0) {
		list.remove(i);
	}else {
		i++;
	}
}
```









##### 参考资料

[java ArrayList.remove()的三种错误用法以及六种正确用法](https://blog.csdn.net/king0406/article/details/103789459)