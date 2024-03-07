# Sentinel

### 1）下载

sentinel官方提供了UI控制台，方便我们对系统做限流设置。大家可以在[GitHub](https://github.com/alibaba/Sentinel/releases)下载。

我这里目前最新的版本是1.8.7

![image-20240304204653304](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403042046523.png)



### 2）运行

将jar包放到任意非中文目录，执行命令：

```sh
java -jar sentinel-dashboard-1.8.1.jar
```

如果要修改Sentinel的默认端口、账户、密码，可以通过下列配置：

| **配置项**                       | **默认值** | **说明**   |
| -------------------------------- | ---------- | ---------- |
| server.port                      | 8080       | 服务端口   |
| sentinel.dashboard.auth.username | sentinel   | 默认用户名 |
| sentinel.dashboard.auth.password | sentinel   | 默认密码   |

例如，修改端口：

```sh
nohup java -Dserver.port=8090 -Dsentinel.dashboard.auth.username=root -Dsentinel.dashboard.auth.password=123456 -jar sentinel-dashboard-1.8.7.jar > out.log 2>&1 &

```





### 3）访问

访问http://localhost:8080页面，就可以看到sentinel的控制台了：

![image-20210715190827846](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403042045131.png)

需要输入账号和密码，默认都是：sentinel



登录后，发现一片空白，什么都没有：

![image-20210715191134448](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403042045158.png)

这是因为我们还没有与微服务整合。

