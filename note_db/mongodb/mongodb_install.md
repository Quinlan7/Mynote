# mongoDB

*了解一下mongoDB*

## 一、安装 mongoDB

*数据库下载*

### 1. 下载

新版的mongodb官网很难找到这个下载离线安装包的地方，因为现在mongodb主推的是他的 云平台数据库 mongodb atlas，点击下面链接进入，然后选择 select package

[mongodb官网下载](https://www.mongodb.com/try/download/community-kubernetes-operator)

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311071414341.png" alt="image-20231107141403238" style="zoom:50%;" />

选择合适的安装包版本，注意这个 Package ，选择 tgz 就是linux的压缩包。我们可以选择直接下载压缩包到本地，也可以复制下载链接，在 linux 机器上使用下面命令直接下载压缩包到服务器上。由于我这里的服务器网速非常慢，所以我直接下载到本地，离线安装。

```shell
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-7.0.2.tgz
```



<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311071415047.png" alt="image-20231107141501973" style="zoom:80%;" />

### 2. 安装

+ 进入到压缩包所在文件夹，使用下面命令解压

```
[root@glnode07 softwares]# tar -zxvf mongodb-linux-x86_64-rhel70-7.0.2.tgz
```

+ 将文件夹移动到你自己的文件夹

```
[root@glnode07 softwares]# mv mongodb-linux-x86_64-rhel70-7.0.2 /export/servers/mongodb-7.0.2
```



### 3. 环境变量

+ 配置环境变量

```
[root@glnode07 servers]# vim /etc/profile
```

```shell
#mongo DB
export MONGODB_HOME=/export/servers/mongoDB7.0.2
export PATH=$PATH:$MONGODB_HOME/bin
```

使环境变量生效

```shell
[root@glnode07 servers]# source /etc/profile
```



### 4. 创建数据和日志目录

```
[root@glnode07 servers]# mkdir -p /home/mongodb/data/{db,logs}
```



### 5. 配置文件

进入到 mongodb 的 bin 目录下，创建配置文件，并编辑

```
[root@glnode07 bin]# touch mongod.conf
[root@glnode07 bin]# vim mongod.conf
```

添加如下内容

```yaml
#数据库数据存放目录
systemLog:
 destination: file
 path: /home/mongodb/data/logs/mongodb.log
 logAppend: true
storage:
 dbPath: /home/mongodb/data/db
net:
 bindIp: 0.0.0.0
 port: 27017
 maxIncomingConnections: 5000
processManagement:
 fork: true
```



### 6. 启动与停止服务

启动服务

```
[root@glnode07 bin]# ./mongod -f ./mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 28910
child process started successfully, parent exiting
```

查看服务

````
[root@glnode07 bin]# netstat -tlnp | grep mongod
tcp        0      0 0.0.0.0:27017           0.0.0.0:*               LISTEN      28910/./mongod 
````

停止服务

```
[root@glnode07 bin]# ./mongod -f ./mongod.conf --shutdown
```



### 7. 使用工具连接

datagrip

![image-20231107160951512](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311071609637.png)

可以看到三个系统数据库

![image-20231107161020307](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311071610379.png)



这个安装完之后我们是没有 mongodb 的命令行的，我们需要单独下载 mongodb shell 才可以使用mogodb的命令行工具。

或者我们可以直接使用 navicat 的命令行，或者 datagrip 的命令行都可以，要是想在服务器上使用命令行就需要下载 mongodb shell 了。





## 二、安装 mongoDB shell

*数据库命令行连接工具*

### 1.下载

[mongoDB shell 官网](https://www.mongodb.com/try/download/shell)

选择合适的版本下载压缩包，离线安装

<img src="https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311071702050.png" alt="image-20231107170250932" style="zoom:80%;" />



### 2. 安装

解压

```
[root@glnode07 softwares]# tar -zxvf mongosh-2.0.2-linux-x64.tgz
```

移动位置并重命名

```
[root@glnode07 softwares]# mv mongosh-2.0.2-linux-x64 /export/servers/mongosh2.0.2
```



### 3. 使用

进入到bin目录下使用下面命令

```
[root@glnode07 bin]# ./mongosh
```

连接成功

![image-20231107170957631](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311071709784.png)