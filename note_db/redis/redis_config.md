# redis 配置文件修改

*总结一些关于redis配置文件的常用修改*

## 基础配置



### 后台启动redis

1. 打开redis的配置文件，找到 daemonize 配置，修改为yes

![image-20240311113000414](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403111130542.png)

如果是使用vim编辑器的话，可以使用 `/daemonize` 来找到所有匹配选项，使用n切换下一个，使用`:noh`推出查找模式

2. 重启 redis

```
redis-cli shutdown
redis-server ../etc/redis.conf
```



### 开启 redis 网络访问

*如果你的redis启动了，可以使用 redis-cli 连接，但是你的本机无法连接，需要修改两个位置*

1. 关闭保护模式，找到配置中的 protected-mode 修改为 no

![image-20240311113533200](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403111135271.png)

 重启服务后，可以通过，redis命令行，使用命令 config get protected-mode 查看该配置是否生效

2. 修改 bind 配置为 0.0.0.0

![image-20240311113747002](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403111137058.png)

> 在 Redis 配置文件中，`bind` 配置项指定了 Redis 服务器绑定的网络接口地址。
>
> 默认情况下，`bind` 配置项是被注释掉的，或者被设置为 `bind 127.0.0.1`。这意味着 Redis 只会监听本地回环接口（localhost），即只能本地访问 Redis 服务器。
>
> 如果你希望 Redis 可以通过网络访问，你需要将 `bind` 配置为 Redis 服务器所在主机的 IP 地址，或者设置为 `bind 0.0.0.0`，表示监听所有的网络接口。
>
> 例如，如果你希望 Redis 服务器可以在本地网络中访问，你可以设置 `bind` 为服务器的 IP 地址：
>
> ```
> bind 192.168.1.100
> ```
>
> 或者，如果你希望 Redis 服务器对所有的网络接口都开放，可以设置为：
>
> ```
> bind 0.0.0.0
> ```
>
> 需要注意的是，为了安全起见，应该尽量限制 Redis 服务器可以访问的网络接口，避免暴露在公共网络中。



### 配置日志文件存储位置

在Linux系统中，数据目录通常位于`/var/lib/redis/`或`/usr/local/redis/`。在Windows系统中，数据目录通常位于`C:\Program Files\redis\data`。日志文件默认位置是不会存储在我们的 redis 的安装目录的。我们可以通过配置文件配置日志目录。

在配置文件中的 logfile 中，输入你的日志文件的绝对路径。

![image-20240311121658564](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403111216632.png)





## RDB 持久化相关配置

Redis内部的触发RDB的机制，格式如下：

```properties
# 900秒内，如果至少有1个key被修改，则执行bgsave ， 如果是save "" 则表示禁用RDB
save 900 1  
save 300 10  
save 60 10000 
```



RDB的其它配置也可以在redis.conf文件中设置：

```properties
# 是否压缩 ,建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes

# RDB文件名称
dbfilename dump.rdb  

# 文件保存的路径目录
dir ./ 
```





## AOF 持久化相关配置

1. 关闭 RDB 持久化配置，将 save 配置为 空

![image-20240311125658357](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403111256437.png)

2. AOF默认是关闭的，需要修改redis.conf配置文件来开启AOF：

```properties
# 是否开启AOF功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
# AOF文件夹名称
appenddirname "appendonlydir"
```

3. AOF的命令记录的频率也可以通过redis.conf文件来配：

```properties
# 表示每执行一次写命令，立即记录到AOF文件
appendfsync always 
# 写命令执行完先放入AOF缓冲区，然后表示每隔1秒将缓冲区数据写到AOF文件，是默认方案
appendfsync everysec 
# 写命令执行完先放入AOF缓冲区，由操作系统决定何时将缓冲区内容写回磁盘
appendfsync no
```

4. Redis也会在触发阈值时自动去重写AOF文件。阈值也可以在redis.conf中配置：

```properties
# AOF文件比上次文件 增长超过多少百分比则触发重写
auto-aof-rewrite-percentage 100
# AOF文件体积最小多大以上才触发重写 
auto-aof-rewrite-min-size 64mb 
```

