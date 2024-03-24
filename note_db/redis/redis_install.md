# Redis安装



### Redis安装与启动windows服务

[Redis 安装](https://www.runoob.com/redis/redis-install.html)

这样安装完在系统服务中并没有redis服务

[redis服务启动](https://blog.51cto.com/YangPC/5483487)





### Redis安装与启动Linux服务

##### 1.下载压缩包到服务器

我下载的是最新版本7.0.12，这里我是直接下载到了root目录下

```
wget https://github.com/redis/redis/archive/7.0.12.tar.gz
```

出现如下画面

![image-20240311093903915](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403110939006.png)



##### 2.解压

将压缩文件解压，输入以下命令解压到当前目录

```
tar -zvxf 7.0.12.tar.gz
```

使用ls查看文件

![image-20240311093914311](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403110939357.png)

##### 3.移动文件夹

一般都是下载到/usr/local/redis，所以我们直接把文件夹移动到这个目录

```
mv /root/redis-5.0.7 /usr/local/redis
```



##### 4.编译 and 安装

进入到redis文件夹下，看到有Makefile文件，输入命令make执行编译命令，接下来控制台会输出各种编译过程中输出的内容。

```
cd redis-4.0.8
make
cd src
make install PREFIX=/usr/local/redis
```

这里多了一个关键字 **`PREFIX=`** 这个关键字的作用是编译的时候用于指定程序存放的路径。比如我们现在就是指定了redis必须存放在/usr/local/redis目录。假设不添加该关键字Linux会将可执行文件存放在/usr/local/bin目录，

库文件会存放在/usr/local/lib目录。配置文件会存放在/usr/local/etc目录。其他的资源文件会存放在usr/local/share目录。这里指定好目录也方便后续的卸载，后续直接rm -rf /usr/local/redis 即可删除redis。



##### 5.移动配置文件到安装目录下

```
cd ../
mkdir /usr/local/redis/etc
mv redis.conf /usr/local/redis/etc
```



##### 6.配置redis为后台启动

将配置文件中的daemonize no 改成daemonize yes

```
cd etc/
vim redis.conf
```

![image-20240311093922262](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403110939319.png)

通过 /daemonize 查找到属性，默认是no，更改为yes即可。 (vim可以通过`/关键字`查找出现多个结果则使用 n字符切换到下一个即可，查找到结果后输入:noh退回到正常模式)



##### 7.将redis加入到开机启动

```
vi /etc/rc.local //在里面添加内容：/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf (意思就是开机调用这段开启redis的命令)
```



##### 8.开启redis

```
/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf 
```



##### 9.将redis-cli,redis-server拷贝到bin下，让redis-cli指令可以在任意目录下直接使用

```
cp /usr/local/redis/bin/redis-server /usr/local/bin/
cp /usr/local/redis/bin/redis-cli /usr/local/bin/
```



##### 10.让防火墙通过（外网可以访问）

防火墙可能每个Linux使用的不同，所以这里不给出具体命令，请自行百度

```
a.配置防火墙:  


b.虽然防火墙开放了6379端口，但是外网还是无法访问的，因为redis监听的是127.0.0.1：6379，并不监听外网的请求。

（一）把文件夹目录里的redis.conf配置文件里的bind 127.0.0.1前面加#注释掉

（二）命令：redis-cli连接到redis后，通过 config get  daemonize和config get  protected-mode 是不是都为no，如果不是，就用config set 配置名 属性 改为no。
```



##### 11.使用工具连接redis

我使用的是another redis desktop manager，用户名密码默认空

![image-20240311093928870](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403110939967.png)

成功连接

![image-20240311093935977](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403110939180.png)





##### 参考

[Linux安装部署Redis(超级详细)](https://www.cnblogs.com/hunanzp/p/12304622.html)

[linux 安装redis 完整步骤](https://juejin.cn/post/7012898467643621412)









### redis问题

（二）命令：redis-cli连接到redis后，通过 config get  daemonize和config get  protected-mode 是不是都为no，如果不是，就用config set 配置名 属性 改为no。

```
redis-cli shutdown 
redis-server &



config get requirepass

#指定配置文件q
/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf 
```







### 启动报错 `GLIBC_2.27' not found

```sh
[root@glnode06 bin]# ./redis-server ../etc/redis.conf 
./redis-server: /lib64/libc.so.6: version `GLIBC_2.27' not found (required by ./redis-server)
./redis-server: /lib64/libc.so.6: version `GLIBC_2.28' not found (required by ./redis-server)
```

之后尝试把源代码复制过去，编译发现也报错

```sh
[root@glnode06 redis-7.2.4]# make
cd src && make all
which: no python3 in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/java/default/bin:/export/servers/Hive3.1.2/bin:/export/servers/Hadoop3.3.4/sbin:/export/servers/Hadoop3.3.4/bin:/export/servers/Mysql5.7.39/bin:/export/softwares/redis-5.0.5/bin:/root/bin:/export/servers/mongodb/bin:/usr/java/default/bin:/export/servers/Hive3.1.2/bin:/export/servers/Hadoop3.3.4/sbin:/export/servers/Hadoop3.3.4/bin:/export/servers/Mysql5.7.39/bin:/export/softwares/redis-5.0.5/bin:/root/bin)
make[1]: Entering directory `/export/servers/redis-7.2.4/src'
    GEN commands.def
Processing json files...
Linking container command to subcommands...
Checking all commands...
Generating commands.def...
All done, exiting.
    CC commands.o
    LINK redis-server
/tmp/ccPNne18.ltrans1.ltrans.o: In function `anetPipe':
/export/servers/redis-7.2.4/src/anet.c:676: undefined reference to `fcntl64'
/export/servers/redis-7.2.4/src/anet.c:663: undefined reference to `fcntl64'
/export/servers/redis-7.2.4/src/anet.c:666: undefined reference to `fcntl64'
/export/servers/redis-7.2.4/src/anet.c:672: undefined reference to `fcntl64'
/tmp/ccPNne18.ltrans1.ltrans.o: In function `anetCloexec':
/export/servers/redis-7.2.4/src/anet.c:118: undefined reference to `fcntl64'
/tmp/ccPNne18.ltrans1.ltrans.o:/export/servers/redis-7.2.4/src/anet.c:127: more undefined references to `fcntl64' follow
collect2: error: ld returned 1 exit status
make[1]: *** [redis-server] Error 1
make[1]: Leaving directory `/export/servers/redis-7.2.4/src'
make: *** [all] Error 2

```

这个报错的原因可能是没有下载 tcl，你可以尝试下载后再编译是否可以，但是我的服务器特殊原因没有连接网络，所以尝试别的方法

```
yum install -y tcl
```



https://www.cnblogs.com/FengZeng666/p/15989106.html

我尝试了网上找到的这个方法，可惜报错

These critical programs are missing or too old: make bison compiler
Check the INSTALL file for required versions.

还是需要联网下载，gg
