# Spark HA



Spark Standalone集群是主从架构的集群模式，因此同样存在单点故障问题，解决这个问题就需要用到Zookeeper服务，其基本原理是将Standalone集群连接到同一个Zookeeper实例并启动多个Master节点，利用Zookeeper提供的选举和状态保存功能，可以使一台Master节点被选举，另外一台Master节点处于Standby状态。当活跃的Master发生故障时，Standby状态的Master就会被激活，然后恢复集群调度，整个恢复过程可能需要1-2分钟。



#### 1．修改spark-env.sh配置文件

在spark-env.sh文件中，将指定Master节点的配置参数注释，即在SPARK_MASTER_HOST配置参数前加“#”，表示注释当前行，添加SPARK_DAEMON_JAVA_OPTS配置参数，具体内容如下。

```
#Spark HA 注销掉以下两行
#SPARK_MASTER_HOST=glnode01
#SPARK_MASTER_PORT=7077
#Master 监控页面默认访问端口为 8080，但是可能会和 Zookeeper 冲突，所以改成 8989，也可以自定义，访问 UI 监控页面时请注意
SPARK_MASTER_WEBUI_PORT=8989
export SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=glnode01,glnode02,glnode03 -Dspark.deploy.zookeeper.dir=/spark"
```

#### 2．启动Spark HA集群

在普通模式下启动Spark集群，只需要通过/spark/sbin/start-all.sh一键启动脚本就可以了。然而，在高可用模式下启动Spark集群，首先需要启动Zookeeper集群，然后在任意一台主节点上执行start-all.sh命令启动Spark集群，最后在另外一台主节点上单独启动Master服务。具体步骤如下。

 （1）启动Zookeeper服务

 依次在三台节点上启动Zookeeper，命令如下。

```shell
$ zkServer.sh start
```

 （2）启动Spark集群

 在hadoop01主节点使用一键启动脚本启动，命令如下。

```shell
$ /export/servers/spark/sbin/start-all.sh --master spark://glnode01:7077
```

 （3）单独启动Master节点

 在hadoop02节点再次启动Master服务，命令如下。

```shell
$ /export/servers/spark/sbin/start-master.sh
```

 启动成功后，通过浏览器访问`http://hadoop02:8080`，查看备用Master节点状态，如图1所示。





#### zookeeper集群启动脚本

```bash
#! /bin/sh

for host in glnode01 glnode02 glnode03

do
 	# 打印正在执行的操作
	echo "Starting ZooKeeper on $host..."

    ssh $host "/export/servers/Zookeeper3.5.7/bin/zkServer.sh start"

    # 检查是否成功启动 ZooKeeper
    if [ $? -eq 0 ]; then
        echo "$host: ZooKeeper is running"
    else
        echo "$host: Failed to start ZooKeeper"
    fi

done
```





##### 参考

[Spark HA集群部署](https://book.itheima.net/course/1269935677353533441/1270998166728089602/1270999667882074116)
