# Hadoop



## 一、入门

### 1 软件生态

大数据的核心工作

+ 存储
+ 计算
+ 传输

大数据软件生态

+ 存储：HDFS，HBase（基于HDFS），kudu
+ 计算：MapReduce，hive（基于MapReduce），Spark，Flink
+ 传输：kafka，pulsar，Flume，sqoop

### 2 Hadoop 概述

 Hadoop是Apache软件基金会下的顶级开源项目，用以提供:

+ 分布式数据存储（HDFS）

+ 分布式数据计算（MapReduce）

+ 分布式资源调度（Yarn）

为一体的整体解决方案。



Google三篇论文

+ 《The Google file system》 :谷歌分布式文件系统GFS 
+ 《MapReduce: Simplified Data Processing on Large Clusters》 :谷歌分布式计算框架MapReduce
+ 《Bigtable: A Distributed Storage System for Structured Data》:谷歌结构化数据存储系统

 



## 二、HDFS

### 1 分布式的基础架构

**主从模式（中心化模式）**

+ master
+ slaves

**去中心化模式**

+ 无中心

### 2 HDFS 基础架构

![image-20230904153653467](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309041537709.png)

 

**NameNode :**

+ HDFS系统的主角色，是一个独立的进程
+ 负责管理HDFS整个文件系统
+ 负责管理DataNode

**SecondaryNameNode:**

+ NameNode的辅助，是一个独立进程
+ 主要帮助NameNode完成元数据整理工作(打杂)

**DataNode :**

+ HDFS系统的从角色，是一个独立进程
+ 主要负责数据的存储，即存入数据和取出数据



### 3 HDFS 安装

+ **hadoop目录结构**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309042150369.png" alt="image-2023090421034049" style="zoom:40%;" />

+ **需修改的篇配置文件**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309042204367.png" alt="image-2023094220438251" style="zoom:40%;" />

+ **workers 和 hadoop-env.sh 文件的配置**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309042205220.png" alt="image-2023090422053485" style="zoom:50%;" />

+ **core-site.xml 文件配置**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050815540.png" alt="image-2023090508150369" style="zoom:50%;" />

+ **hdfs-site.xml 文件配置**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050825425.png" alt="image-2023095082512263" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050833461.png" alt="image-202309008326349" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050835035.png" alt="image-202309050835519907" style="zoom:100%;" />

+ **准备数据目录**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050845304.png" alt="image-202300508404364" style="zoom:50%;" />

+ **分发 Hadoop 文件夹（将主节点的hadoop分发的从节点）**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050854149.png" alt="image-20230905085444028" style="zoom:40%;" />

+ **配置环境变量**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050859450.png" alt="image-2023095085903287" style="zoom:40%;" />

+ **授权 hadoop 用户**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050902013.png" alt="image-20230905090236885" style="zoom:40%;" />

+ **格式化整个文件系统**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051116231.png" alt="image-20230905111634094" style="zoom:50%;" />

+ **jps**

![image-20230905153253364](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051532418.png)

+ **查看 HDFS WEBUI**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051117027.png" alt="image-20230905111755920" style="zoom:50%;" />

+ **总结**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309050913332.png" alt="image-2023090509312221" style="zoom:50%;" />



### 4 HDFS 的 Shell 操作

#### 4.1 HDFS 相关进程的启停管理命令

+ **一键启停脚本**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051548631.png" alt="image-20230905154850497" style="zoom:50%;" />

+ **单进程启停（本机）**

![image-20230905160250914](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051602034.png)



#### 4.2 HDFS文件系统操作命令

##### HDFS 文件系统的基本信息

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051716826.png" alt="image-20230905171649654" style="zoom:40%;" />

##### 介绍

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051740383.png" alt="image-2023090517407126" style="zoom:40%;" />

1. 创建文件夹

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051740983.png" alt="image-20230905174052899" style="zoom:50%;" />

2. 查看指定目录下的内容

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051801737.png" alt="image-20230905180147588" style="zoom:40%;" />

3. 上传文件到 HDFS 指定目录下

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051804035.png" alt="image-20230905180433914" style="zoom:40%;" />

4. 查看 HDFS 文件内容

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051807914.png" alt="image-20230905180727810" style="zoom:50%;" />

5. 下载 HDFS 文件

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309051810885.png" alt="image-20230905181009762" style="zoom:50%;" />

6. 拷贝 HDFS 文件

<img src="C:/Users/zhf/AppData/Roaming/Typora/typora-user-images/image-20230905212203911.png" alt="image-2023090521203911" style="zoom:50%;" />

7. 追加数据到 HDFS 文件中

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061013829.png" alt="image-20230906101319641" style="zoom:50%;" />

8. HDFS 数据移动操作

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061015052.png" alt="image-20230906101528966" style="zoom:50%;" />

9. HDFS 数据删除操作

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061034468.png" alt="image-20230906103400336" style="zoom:50%;" />

10. HDFS WEB 浏览

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061037550.png" alt="image-20230906103744414" style="zoom:40%;" />



#### 4.3 HDFS 权限

**HDFS 超级用户**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061053015.png" alt="image-20230906105336850" style="zoom:40%;" />

**修改权限**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061055597.png" alt="image-20230906105500511" style="zoom:50%;" />

**总结**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309061057753.png" alt="image-20230906105749684" style="zoom:50%;" />

#### 4.4 Big Data Tools 插件

+ **配置 windows**

![image-20230907084421683](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309070844792.png)

+ 将服务器上的 hadoop/etc/hadoop/ 文件夹下的所有内容复制到 windows 本地的对应文件夹中

![image-20230907084926742](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309070849888.png)



+ **连接配置**（使用配置文件连接）

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309070843567.png" alt="image-20230907084302434" style="zoom:80%;" />



#### 4.5 HDFS 客户端 NFS （可选）

+ **HDFS NFS 网关**

![image-20230907111121182](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071111348.png)

+ **配置NFS core-site.xml**

![image-20230907111515589](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071115743.png)

+ **配置NFS hdfs-site.xml**

![image-20230907111739237](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071117390.png)

+ **启动 NFS 功能**

![image-20230907112603622](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071126778.png)

+ **检查 NFS 是否正常**

![image-20230907112714066](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071127201.png)

+ **在 Windows 挂在 HDFS 文件系统**

![image-20230907112928066](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071129192.png)

+ **在 Windows 挂载 HDFS 文件系统**

![image-20230907144152808](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071441974.png)



### 5 HDFS 存储原理

#### 5.1 HDFS 存储原理

*在[2023新版黑马程序员大数据入门到实战教程，大数据开发必会的Hadoop、Hive，云平台实战项目全套一网打尽](https://www.bilibili.com/video/BV1WY4y197g7?p=33&vd_source=88553068aea08e911c13f3696d2bfaa2)这套教程中关于 HDFS 的原理理论部分实在是太简单了，要去别的教程多了解一些*

#### 5.2 fsck 命令

##### 1. 配置 HDFS 数据块的副本数量

+ **HDFS 副本块的数量永久配置**

![image-20230907160350643](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071603815.png)

+ **单次修改**

![image-20230907160336811](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071603976.png)

##### 2. fsck 命令查看文件系统状态和验证文件的数据副本

+ **fsck 查看文件副本数等信息**

![image-20230907192212612](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309071922930.png)

```
[hadoop@Slave1 ~]$ hdfs fsck /home/hadoop/city/data/ -files -blocks -locations
Connecting to namenode via http://Master:50070
FSCK started by hadoop (auth:SIMPLE) from /10.1.40.86 for path /home/hadoop/city/data/ at Thu Sep 07 18:54:59 CST 2023
/home/hadoop/city/data/ <dir>
/home/hadoop/city/data/citydata 218 bytes, 1 block(s):  OK
0. BP-1543104021-10.1.40.83-1627559785700:blk_1074210427_469682 len=218 repl=3 [10.1.40.87:50010, 10.1.40.86:50010, 10.1.40.84:50010]

Status: HEALTHY
 Total size:	218 B
 Total dirs:	1
 Total files:	1
 Total symlinks:		0
 Total blocks (validated):	1 (avg. block size 218 B)
 Minimally replicated blocks:	1 (100.0 %)
 Over-replicated blocks:	0 (0.0 %)
 Under-replicated blocks:	0 (0.0 %)
 Mis-replicated blocks:		0 (0.0 %)
 Default replication factor:	3
 Average block replication:	3.0
 Corrupt blocks:		0
 Missing replicas:		0 (0.0 %)
 Number of data-nodes:		3
 Number of racks:		1
FSCK ended at Thu Sep 07 18:54:59 CST 2023 in 1 milliseconds


The filesystem under path '/home/hadoop/city/data/' is HEALTHY
```

+ **block 配置**

![image-20230907222006946](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309072220036.png)

+ 总结

![image-20230907222331408](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309072223472.png)



#### 5.3 NameNode 元数据

##### 1. NameNode 对于 Block 块的管理

![image-20230908093102047](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309080931289.png)

+ **edits 文件**（日志文件）

![image-20230908093419751](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309080934923.png)

+ **fsimage 文件**

![image-20230908095502177](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309080955360.png)

+ **NameNode fsimage 元数据维护**

![image-20230908102203263](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309081022390.png)

+ **元数据和并控制参数配置**

![image-20230908100035449](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309081000557.png)

+ **SecondaryNameNode 的唯一作用**
+ ![image-20230908100326767](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309081003898.png)

+ **总结**
  + HDFS 在查找数据的时候是基于 fsimage 和 edits 的，从 fsimage 和 上次 fsimage 更新时间点之后的 edits 文件，找到最新的文件

![4444](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309081015718.png)



#### 5.4 HDFS 数据的读写流程

##### 1. 数据写入流程

![image-20230909103803004](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309091038283.png)

+ NameNode 不负责写入数据，只负责元数据的记录和权限审批
+ 客户端直接向一台 DataNode 写数据，这个 DataNode 一般是离客户端最近（网络距离）的哪一个
+ 数据块副本的赋值工作，由 DataNode 之间自行完成（构建一个 PipLine，按顺序复制分发）

##### 2. 数据读取流程

![image-20230909110643508](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309091106706.png)

##### 3. 总结

![image-20230909111217609](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309091112749.png)





## 三、MapReduce And Yarn

*分布式计算和资源调度*

### 1 分布式计算概述



























##### 参考

+ [[翻译] HDFS架构](https://juejin.cn/post/7254391275800100925?searchId=202309071510278A2E2041052D9626F272) :hadoop 官网 关于 HDFS 部分的翻译
+ [2023新版黑马程序员大数据入门到实战教程，大数据开发必会的Hadoop、Hive，云平台实战项目全套一网打尽](https://www.bilibili.com/video/BV1WY4y197g7?p=33&vd_source=88553068aea08e911c13f3696d2bfaa2) :b站 教程，偏向于实操
+ 