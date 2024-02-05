# ElasticSearch



## 二、ES 安装

*以最新版本的 ES8.12.0 为基础*

### 1. 下载

[ES官网下载链接](https://www.elastic.co/cn/downloads/elasticsearch)，选择你系统对应的下载包，注意版本

![image-20240121112038988](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211120082.png)

### 2. 安装

将文件上传到服务器，然后解压缩

```
tar -xvf elasticsearch-8.1.0-linux-x86_64.tar.gz 
```

### 3. 创建用户

创建 Elasticsearch用户 并赋予 Elasticsearch安装目录 权限，这是 Elasticsearch 的一种安全措施（不能使用`root`用户）

```
[root@glnode es]# useradd es
[root@glnode es]# passwd es
更改用户 es 的密码 。
新的 密码：
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。
```

修改文件夹权限

```
chown -R es:es /export/servers/elasticsearch-8.12.0
```

切换用户

```
su es
```

### 4. 修改配置并启动

进入到对应的 config 目录，修改配置文件 elasticsearch.yml

```
[es@glnode07 elasticsearch-8.12.0]$ cd config/
[es@glnode07 config]$ vim elasticsearch.yml
```

网络配置修改：最初是全部注释掉的

开启 ES 的远程连接

```yaml
# ---------------------------------- Network -----------------------------------
#
# By default Elasticsearch is only accessible on localhost. Set a different
# address here to expose this node on the network:
#
network.host: 0.0.0.0
#
# By default Elasticsearch listens for HTTP traffic on the first free port it
# finds starting at 9200. Set a specific HTTP port here:
#
http.port: 9200
#
# For more information, consult the network module documentation.

```

然后启动就会报错。。。。。并且在对应平台报错误不同，看后面的常见问题部分进行对应配置修改，如果没有包含你的错误就去 google。

然后 加上 -d 参数，以后台方式启动

```
[es@glnode07 config]$ ./elasticsearch -d
```



### 5. 修改密码

新版的 ES 会自动生成用户和密码：用户名统一为 elastic，随机生成密码

通过下面命令可以修改密码

```
[es@glnode07 config]$ ./bin/elasticsearch-reset-password -u elastic -i
This tool will reset the password of the [elastic] user.
You will be prompted to enter the password.
Please confirm that you would like to continue [y/N]y


Enter password for [elastic]: 
Re-enter password for [elastic]: 
Password for the [elastic] user successfully reset.
```



### 6. 访问web端

https://glnode07:9200/浏览器输入你的ip和9200端口，注意一定是https

![image-20240121121730755](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211217821.png)

第一次需要输入用户名和密码，看到如下页面说明成功

![image-20240121120444324](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211204390.png)



### 7. 取消密码验证和https

在 ES8 中默认配置是 https 访问，并且开启安全验证。可是我们自己做demo时并不像开启，过于麻烦。

+ 修改配置：将以下两项修改为 false 即可

```
xpack.security.enabled: false
xpack.security.http.ssl:
  enabled: false
```

![image-20240121125719947](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211257103.png)

>  关于各个配置项含义可以查看[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html)

+ 然后重启服务。

关闭服务是通过 jps 命令查看 ES 进程号，直接 `kill -9 pid`

+ 验证

重启之后，直接在服务器上 curl 命令即可

> 如果没有关闭 安全认证 或者 使用 https，这一步是会报错的

![image-20240121130229168](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211302271.png)

+ apipost 验证

es 本身就提供了很多 RESTful 风格的接口，我们可以直接访问。

这里我是用 apipost 访问测试

> 若未关闭安全验证，这一步是会报错并提示你需要验证的

![image-20240121130530060](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211305244.png)

可以直接使用http访问，并且无需验证









### 常见问题

1. 没有权限

![image-20240120202346005](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401202023176.png)

修改 ES 文件夹的权限（需要用root用户）

```
chown -R es:es /usr/local/es
```

2. Elastic search error: "Native controller process has stopped - no new native processes can be started"

![image-20240121111149152](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211111319.png)

直接root执行下面命令，问题解决

```shell
sysctl -w vm.max_map_count=262144
```

实际上日志中显示 vm.max_map_count 默认设置为小。这最终导致了消息的级联问题。

3. the default discovery settings are unsuitable for production use; at least one of [ discovery . seed hosts，

这是由于修改远程登陆配置后默认以集群方式启动，所以需要修改为单节点启动

进入 elasticsearch.yml 文件修改配置，因为当前节点名默认为 node-1

![image-20240121114014253](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211140342.png)

```
cluster.initial_master_nodes: ["node-1"]
```









## 三、Kibana 安装

### 1. 简介

`Kibana`是一个针对`Elasticsearch`的`开源分析及可视化平台`，使用Kibana可以`查询、查看并与存储在ES索引的数据进行交互操作`，使用Kibana能执行高级的`数据分析，并能以图表、表格和地图的形式查看数据。`

### 2. 下载

[官网下载链接](https://www.elastic.co/cn/downloads/kibana)，注意选择的版本一定要和 ES 相同版本，完全相同的版本号

![image-20240121125909116](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211259215.png)



### 3. 安装

同样上传到服务器，并且解压

```
[es@glnode07 servers]$ tar -zxvf kibana-8.12.0-linux-x86_64.tar.gz
```

然后修改权限

```
[root@glnode07 servers]# chown -R es:es /export/servers/kibana-8.12.0
```

### 4. 修改配置

配置允许远程连接

```
server.host: "0.0.0.0"
```

配置 ES 连接地址

```
elasticsearch.hosts: ["http://localhost:9200"]
```

### 5. 后台启动

使用以下命令可以在后台启动

```
[es@glnode07 bin]$ nohup ./kibana &
```

由于我们之前配置了，取消安全验证，所以启动后直接访问 http://glnode07:5601 即可

![image-20240121141244473](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401211412597.png)

如果没有配置取消安全验证，会需要输入一个 token 和 code

### 6. 结束kibana

```
[root@glnode07 config]# netstat -anltp|grep 5601
tcp        0      0 0.0.0.0:5601            0.0.0.0:*               LISTEN      20913/./../node/bin 
[root@glnode07 config]# kill -9 20913
```



## 四、docker方式安装

*安装低版本的 es，因为新版涉及安全验证等，很麻烦，主要原因是新版对应的IK分词器还没有发布呢。。。。*

> 关于docker安装可以查看文章[docker安装并配置外网代理](https://blog.csdn.net/miserable_world/article/details/135691888?spm=1001.2014.3001.5501)

### 1. 创建网络

```
[root@glnode08 ~]# docker network create es-net
287b738b43d3eb35787b0d1a531ea375caed28736ecbd66b17c655f07281f214
```

### 2. 下载

下载的版本为 7.12.1，因为 ES8 以上的版本中包含默认的安全配置，使用docker安装的话，修改此部分配置格外麻烦，并且我尝试了安装 8.12.0 的版本，如果docker运行时你的 ES 取消了安全配置，可是在使用 kibana 时，依然会有安全配置问题（具体为登录5601端口是依然需要你提供token，可是我的es已经取消了安全认证，无法使用命令生成token），使用传统方式安装则只需要取消 ES 的安全认证，kibana 就可以直接使用。

```
[root@glnode08 ~]# docker pull kibana:7.12.1
[root@glnode08 ~]# docker pull elasticsearch:7.12.1
```

### 3. 运行

先创建对应的挂载目录

```markdown
#1、创建Elasticsearch配置文件夹
mkdir -p /home/elasticsearch/config

#2、创建Elasticsearch数据文件夹
mkdir -p /home/elasticsearch/es-data

#3、创建Elasticsearch插件文件夹（如：ik）
mkdir -p /home/elasticsearch/es-plugins

#说明：目的将CentOS本地的文件夹映射到Elasticsearch容器，以实现容器数据的持久化到CentOS本地，以及通过CentOS本地文件夹内容的修改同步到容器

#创建并写入elasticsearch.yml配置，注意：http.host: 0.0.0.0 冒号后有一空格
echo "http.host: 0.0.0.0">>/home/elasticsearch/config/elasticsearch.yml
```

修改权限==一定要修改权限，不然后面会有问题==

```shell
chmod -R 777 /usr/local/data-docker/elasticsearch/
```

es 运行命令

```shell
docker run -d \
	--name es \
	-e "discovery.type=single-node" \
	-v /home/elasticsearch/es-data:/usr/share/elasticsearch/data \
	-v /home/elasticsearch/es-plugins:/usr/share/elasticsearch/plugins \
	-v /home/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
	--privileged \
	--network es-net \
	-p 9200:9200 \
	-p 9300:9300 \
	elasticsearch:7.12.1
```

kibana 运行命令

```shell
docker run -d \
	--name kibana \
	-e ELASTICSEARCH_HOSTS=http://es:9200 \
	--network=es-net \
	-p 5601:5601 \
	kibana:7.12.1
```

可以访问对应的地址测试

### 4. 安装 IK 分词器

[IK 分词器](https://github.com/medcl/elasticsearch-analysis-ik/releases)，用于支持智能化的中文分词，一定要找到和你的 es 版本完全相同的版本号。离线下载下来，放入你的对应的插件挂在目录即可

#### 4.1 查看对应数据卷

```
[root@glnode08 es-plugins]# docker inspect -f '{{ .Mounts }}' es
[{bind  /home/elasticsearch/es-data /usr/share/elasticsearch/data   true rprivate} {bind  /home/elasticsearch/es-plugins /usr/share/elasticsearch/plugins   true rprivate}]
```

#### 4.2 解压并上传到挂载目录

可以看到我们对应的本地挂在目录，在/home/elasticsearch/es-plugins/目录下创建ik目录，并且把解压的文件全部放进去，然后重启容器

效果如下

![image-20240123182555680](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401231825820.png)

#### 4.3 重启容器

```shell
docker restart es
```

#### 4.4 测试分词器

```
POST /_analyze
{
  "text": "亚索学习java太棒了",
  "analyzer": "standard"
}

POST /_analyze
{
  "text": "亚索学习java太棒了",
  "analyzer": "ik_smart"
}
```

![image-20240123182912820](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401231829187.png)

可以正确分词了！







### 参考

[Elastic search error: "Native controller process has stopped - no new native processes can be started"](https://stackoverflow.com/questions/60182669/elastic-search-error-native-controller-process-has-stopped-no-new-native-pro)

[ES8安装](https://juejin.cn/post/7264951443345604645)
