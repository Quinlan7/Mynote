![image-20231007190152183](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310071901378.png)

# Spark

## 第一章、Spark 基础入门

### 一：Spark 框架概述

#### 1 Spark是什么

![image-20230922200841615](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309222008395.png)



#### 2 Spark & Hadoop（MapReduce）

![image-20230922202416173](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309222024628.png)



#### 3 Spark 的运行模式

![image-20230923214832895](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309232148252.png)

#### 4 Spark 的架构角色

![image-20230922205508840](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309222055692.png)



### 二：Spark 环境搭建 - Local



### 三：Spark 环境搭建 - StandAlone

#### 1 StandAlone 架构

![image-20230923220546723](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309232205408.png)

#### 2 安装 StandAlone 环境



#### 3 测试

#### 4 Spark 应用架构

#### 5 WEB UI 监控

### 四：Spark 环境搭建 - StandAlone - HA

### 五：Spark 环境搭建 - Spark On YARN

*==重中之重==：这是正真的重点，因为一般的 Spark 都会部署在 Yarn上*

> 因为部署在 YARN 上时，我们就不需要部署 Spark StandAlone 集群了，只需要在一台机器上部署 Spark 即可，任务会提交到 YARN 去运行

#### 1 SparkOnYARN 本质

![image-20230924171715804](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309241717196.png)

#### 2 配置 Spark On YARN 环境

确保在 spark -env.sh 中配置了这两个参数即可

```
## HADOOP软件配置文件目录，读取HDFS.上文件和运行YARN集群
HADOOP_CONF_DIR=/export/server/hadoop/etc/hadoop
YARN_CONF_DIR=/export/server/hadoop/etc/hadoop
```



#### 3 部署模式 DeployMode

+ Cluster 模式

![image-20221027203305332](C:/Users/zhf/Desktop/image-20221027203305332.png)



+ Client 模式

![image-20220113171831087](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309241903215.png)



[[Spark on Yarn 两种模式执行流程](http://www.fblinux.com/?p=2457)](http://www.fblinux.com/?p=2457)

#### 4 Spark On YARN 两种模式总结

![image-20230924191436050](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309241914310.png)

#### 5 两种模式详细流程

+ Client 模式

![image-20230924191907940](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309241919359.png)

+ Cluster 模式

![image-20230924192118163](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309241921892.png)



### 六：PySpark 安装





### 七：本机开发环境搭建

#### WordCount 代码

![image-20231007173123026](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310071731156.png)

#### WordCount 代码原理分析

![image-20231007172939018](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310071729317.png)





### 八：分布式代码执行分析

#### 1. Spark 集群角色回顾（Yarn）

![image-20231007174757475](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310071747749.png)

####   2. 分布式代码执行分析

![image-20231007175726289](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310071757437.png)



#### 3. Python On Spark 执行原理

![image-20231007180850823](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310071808056.png)





## 第二章、Spark Core

### 一：RDD 详解

#### 1. RDD 定义

![image-20231007202847758](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310072028962.png)

#### 2. RDD 五大特性

+ RDD 是有分区的
+ 计算函数会作用在每一个分区
+ RDD 之间有相互关联
+ KV 型 RDD 可以有分区器
+ RDD 分区数据的读取会尽量靠近数据所在地（移动数据不如移动计算）

**特性1：RDD是有分区的**

![image-20231008104511426](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081045575.png)

**特性2：RDD的方法会作用在其所有分区上**

![image-20231008110026775](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081100072.png)

**特性3：RDD之间是有依赖关系的（RDD有血缘关系）**

+ 在spark中，总是由一个 RDD 生成另一个 RDD ，再由这个新 RDD 生成 更新的 RDD ，一直迭代这个过程，完成我们的计算任务

![image-20231008110421775](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081104979.png)

**特性4：Key-Value 型的 RDD 可以有分区器**

![image-20231008135124481](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081351657.png)

**特性五：RDD的分区规划，会尽量靠近数据所在的服务器**

![image-20231008135756255](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081357376.png)



#### 3. WordCount 结合 RDD 代码执行分析

![image-20231008141005593](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081410714.png)



### 二：RDD 编程入门

#### 1. 程序入口 SparkContext 对象

 ![image-20231008181724072](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310081817410.png)

#### 2. RDD 的创建

+ 并行化集合创建（本地对象转分布式RDD）



+ 读取外部数据源

#### 3. 算子

#### 4. 常用 Transformation 算子

#### 5. 常用 Action 算子

#### 6. 分区操作算子（Transformation & Action）

#### 7. 面试题：groupByKey 和 GroupByKey 的区别





### 三：RDD 持久化

### 四：RDD 案例

### 五：RDD 共享变量

### 六： Spark 内核调度





## 第三章、Spark SQL

### 一：快速入门

+ SparkSQL 是 Spark 的一个模块，用于处理海量 **结构化数据**
+ RDD 可以处理结构化，半结构化，非结构化数据
+ Spark SQL 的应用

![image-20231009112708181](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091127339.png)

+ Spark SQL 特点

![image-20231009113011938](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091130121.png)

**总结**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091315575.png" alt="image-20231009131539463" style="zoom:50%;" />

### 二：SparkSQL 概述

#### 1. SparkSQL 和 Hive 的异同

![image-20231009132118538](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091321772.png)





#### 2. SparkSQL 的数据抽象

![image-20231009132340077](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091323319.png)

![image-20231009132506429](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091325708.png)





#### 3. DataFrame 数据抽象

![image-20231009132739355](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091327593.png)





![image-20231009133037967](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091330237.png)

#### 4. SparkSession 对象

![image-20231009133425892](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091334055.png)

+ 实践一下

```python
# coding:utf8

# SparkSession 对象的导包，对象是来自于 pyspark.sql 包中
from pyspark.sql import SparkSession

if __name__ == '__main__':
    #  构建SparkSession执行环境入口对象
    spark = SparkSession.builder.\
        appName("test").\
        master("local[*]").\
        getOrCreate()

    #  通过SparkSession对象 获取 SparkContext 对象

    sc = spark.sparkContext

    #  SparkSQL的 HelloWorld
    dataframe = spark.read.csv("../data/input/stu_score.txt", sep=',', header=False)
    dataframe2 = dataframe.toDF("id", "name", "score")
    dataframe2.printSchema()
    dataframe2.show()


    dataframe2.createTempView("score")

    #  SQL 风格

    spark.sql("""
        SELECT * FROM score WHERE name='语文' LIMIT 5
    """).show()

    #  DSL 风格
    dataframe2.where("name='语文'").limit(8).show()
```



#### 5. 总结

![image-20231009143518318](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310091435516.png)

### 三：DataFrame 入门和操作

 



 

### 四：SparkSQL 函数定义

### 五：SparkSQL 的运行流程

### 六：SparkSQL 整合 Hive

#### 1. 原理

> Spark SQL 提供了类似于 Hive 的功能，但是 Spark 没有提供元数据的管理功能，所以 Spark On Hve 就是 Spaark 借用了 Hive 的 metastore 功能

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310111053897.png" alt="image-20231011105327599" style="zoom:80%;" />

#### 2. 配置

+ hive-site.xml 配置文件

> 直接把 Hive 的 conf 目录下的 hive-site.xml 复制到 Spark 的 conf 目录下就可以

+ mysql 驱动（metastore 源数据存储在mysql中）

> 把 mysql 的驱动 jar 包下载到 Spark 的 jars 目录下就可以

#### 3. 在代码中集成

 





### 七：分布式 SQL 引擎配置

#### 1. 概念

> spark 和 Hive 一样，提供了一个类似于 hiveserver2 的 JDBC 连接工具 thriftserver（守护进程 daemon），一般的开发人员可以直接通过 JDBC 连接上我们的 Spark ，可以查看 hive 的元数据，通过 SparkSQL 可以直接进行计算，并且操作存储再 HDFS 中的数据。

![image-20231011160430649](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310111604896.png)

#### 2. 客户端工具连接



#### 3. 代码 JDBC 连接





#### 4. 总结

![image-20231011164221512](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310111642692.png)

## 第四章、综合案例



## 第五章、Spark 核心原理















##### 参考

[黑马程序员Spark全套视频教程，4天spark3.2快速入门到精通，基于Python语言的spark教程](https://www.bilibili.com/video/BV1Jq4y1z7VP/?vd_source=88553068aea08e911c13f3696d2bfaa2)

[BigData-Notes](https://github.com/heibaiying/BigData-Notes)