# squid

*71 服务器可以访问外网，其他服务器不能，用这个用具访问外网*

+ 先查看 `/root` 下的 proxy 文件 配置

![image-20231006224119619](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310062241670.png)

+ 一次性开启本次会话的访问外网服务：除了 71 以外的服务器的 `/root` 目录下的 proxy 文件

```
. proxy
proxyon
```

