# Hadoop HA

> 对 core-site.xml 、yarn-site.xml 、dfs-site.xml 做一些配置就可以



## ZKFailoverController

> ZKFC 故障转移控制器

### DFSZKFailoverController

> 这是用于 HDFS 的故障转移控制器
>
> 由于 HDFS 是用于数据底层存储的，所以保障 HDFS 就显得非常重要了，所以 HDFS 的 ZKFC 是一个单独的进程，而 YARN 的 ZKFC 只是一个线程！

![image-20231014212443678](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310142124802.png)

 简单描述下这张图：

​	首先，NameNode（NN）、DataNode（DN）均是HDFS中的组件或节点，NN有两个，分为Active和Standby状态，DN有多个，DN周期性向Active和Standby做数据块汇报；
​    
​	其次，处于中间位置的，实现HDFS NameNode HA最关键的组件是FailoverController，它是运行在NameNode节点上的守护进程，将Active NN节点的信息通过竞争的方式注册到ZooKeeper上，并实现以下三个基本功能：
1）有一个后台线程，周期性的检查NameNode的健康状况，即粗的红色拐弯箭头的Monitor Health，触发状态选举等；
​    
2）定期保持与ZooKeeper的心跳，完成Leader选举（即Active节点选举）和检查Active NN ZNode节点状态等工作，即最上面与ZooKeeper交互的蓝色箭头；
​    
3）向NameNode发送一些命令，完成状态切换。



>
> "DFSZKFailoverController" 是与 Apache Hadoop 中的 Hadoop HDFS (Hadoop Distributed File System) 的故障容错机制相关的组件。具体来说，这是 Hadoop 中的一个类或组件，用于实现 HDFS 的故障容错功能，以确保 HDFS 中的数据可靠性和高可用性。
>
> Hadoop HDFS 是 Hadoop 生态系统的一部分，用于存储大规模数据并提供高容错性的分布式存储解决方案。DFSZKFailoverController 使用 Apache ZooKeeper 来协调 NameNode 主备节点之间的故障切换，以确保 HDFS 的连续性和可用性。它用于监控 NameNode 的状态，并在主节点故障时触发切换到备用节点以继续提供数据服务。
>
> DFSZKFailoverController 是 HDFS 高可用性配置中的一部分，它帮助确保在主 NameNode 失效时，HDFS 仍然可以提供服务，并且数据不会丢失。这是 Hadoop 集群中用于实现 HDFS 高可用性的关键组件之一。通过与 ZooKeeper 集成，它能够维护主备 NameNode 的状态信息，确保故障切换过程的正确性和可靠性。





+ ZKFC 进程如何启动？

> ZKFC可以通过单独的命令启动

```
hdfs zkfc -formatZK     #格式化Zookeeper
hdfs --daemon start zkfc     #三台机器都要执行
```

> 但是，其实 ZKFC 可以自动启动，如果我们配置文件中配置了 HA ，那么 ZKFC 会在启动 hdfs 集群的时候自动启动的
>
> ZKFC 需要在每一台namdnode上启动

![image-20231014214106021](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310142141217.png)







### RM 故障转移控制器

> 用于 YARN 的 ZKFC 故障转移控制器，属于 RM 自己的线程，完全不需要显示的启动命令，只需要完成yarn HA 配置然后启动 yarn 就可以

+ **为什么在Yarn中的ZKFC是线程级别的？为什么架构设计没有HDFS那么复杂？**

因为Yarn主要是用来跑job任务，如果job挂了可以重新跑一次；然而数据是不可以丢失的，在HDFS是进程级别的，保证数据的高可靠性







##### 参考

[（超详细）基于Zookeeper的Hadoop HA集群的搭建](https://blog.csdn.net/JunLeon/article/details/120689889)

[Hadoop-2.7.0中HDFS NameNode HA实现之DFSZKFailoverController、ZKFailoverController（一）](https://blog.csdn.net/lipeng_bigdata/article/details/53638017)

[Hadoop-2.7.0中HDFS NameNode HA实现综述](https://blog.csdn.net/lipeng_bigdata/article/details/53637562?spm=1001.2014.3001.5502)

[ZKFC原理解析](https://blog.csdn.net/answer100answer/article/details/113590690)

[ResourceManager HA 原理](https://blog.csdn.net/ATYtian/article/details/130597640)

[浅析YARN-RM的HA](https://justdodt.github.io/2018/04/10/YARN-RM%E7%9A%84HA/)

[高可用性的ResourceManager](https://blog.csdn.net/ZH519080/article/details/84899597)