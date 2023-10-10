# hive

## 一、Apache Hive 概述

+ **Hive 把 HDFS 映射为关系型数据库，把 SQL 翻译为 MapReduce**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212044409.png" alt="image-20230921204446201" style="zoom:50%;" />





## 二、模拟实现 Hive 功能

![image-20230921205304118](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212053423.png)





## 三、Hive 基础架构

![image-20230921211050333](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212110758.png)





## 四、Hive 部署

**1 下载 mysql 数据库**



**2 配置 Hadoop**

![image-20230921211831188](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212118622.png)

**3 下载解压 Hive**

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212120267.png" alt="image-20230921212004862" style="zoom:50%;" />

**4 下载 MySQL Driver 驱动包**

![](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212120542.png)

**5 配置 Hive**

![image-20230921212739883](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212127066.png)

![image-20230921212650463](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212126739.png)

**6 初始化元数据库**

![image-20230921213228037](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212132214.png)

**7 启动 Hive**

*使用 hadoop 用户，需要先修改文件夹的权限*

![image-20230921213625572](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212136884.png)

**总结**

![image-20230921213732932](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309212137102.png)





## 五、Hive 客户端

### 5.1 HiveServer2 & beeline

+ **hive 客户端体系**

![image-20230922095206176](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309220952409.png)



+ **启动 hiveserver2 服务**

![image-20230922100212678](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309221002816.png)

+ **beeline**![image-20230922101429445](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202309221014975.png)





### 5.2 DataGrip & DBeaver



















### 参考

[2023新版黑马程序员大数据入门到实战教程，大数据开发必会的Hadoop、Hive，云平台实战项目全套一网](https://www.bilibili.com/video/BV1WY4y197g7?p=52&vd_source=88553068aea08e911c13f3696d2bfaa2)

