# Quorum Journal Node



### 1.在HADOOP扮演的角色

JournalNode是在 HDFS 中新加的,journalNode的作用是存放EditLog的,

在非 HA 中 editlog 是和 fsimage 存放在一起的，就存放在 NameNode 结点中，然后 SecondNamenode 做定期合并, HDFS HA 模式下就不需要 SecondNamanode 了，editlog 和 fsimage 存储在 active 的 NameNode 和 Journal Node 中。

![img](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310161051079.png)

 

 Active Namenode与StandBy Namenode之间的就是JournalNode,作用相当于NFS共享文件系统.Active Namenode往里写editlog数据,StandBy再从里面读取数据进行同步.

配置文件是；hdfs-site.xml文件负责

![img](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310161051934.png)

 

 最后进程JPS如下图：

![img](https://img2018.cnblogs.com/blog/1353331/201910/1353331-20191008160209638-2002980631.png)

 

### 2.作用

两个 NameNode 为了数据同步，会通过一组称作 JournalNodes 的独立进程进行相互通信。当 active 状态的 NameNode 的命名空间有任何修改时，会告知大部分的 JournalNodes 进程。

standby 状态的 NameNode 有能力读取 JNs 中的变更信息，并且一直监控 edit log 的变化，把变化应用于自己的命名空间。standby 可以确保在集群出错时，命名空间状态已经完全同步了。

 ![img](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310161051033.png)

 

### 3.资源配置

NameNode 服务器：运行 NameNode 的服务器应该有相同的硬件配置。

 JournalNode 服务器：运行的 JournalNode 进程非常轻量，可以部署在其他的服务器上。注意：必须允许至少3个节点。当然可以运行更多，但是必须是奇数个，如3、5、7、9个等等。

当运行N个节点时，系统可以容忍至少(N-1)/2(N至少为3)个节点失败而不影响正常运行。 

在HA集群中，standby 状态的 NameNode 可以完成 checkpoint 操作，因此没必要配置Secondary NameNode、CheckpointNode、BackupNode。如果真的配置了，还会报错。









##### 参考

[hadoop中的JournalNode](https://www.cnblogs.com/wqbin/p/11635964.html)