# MAVEN



### 远程仓库下载jar包

*在 spring boot 开发时经常会遇到这种问题：有的依赖 MAVEN 无法自动下载，这时就需要我们自己去 MAVEN 中央仓库下载，并 install 后才可以使用*

#### 1. 先到 MAVEN 中央仓库找到我们需要的依赖并下载 jar 包

下载我们需要的版本的 jar 包，到任意一个文件夹下

![image-20230812160416779](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202308121604890.png)

![image-20230812160438927](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202308121604148.png)

#### 2. MAVEN install

在对应文件夹下打开cmd，输入以下命令

```
mvn install:install-file -Dfile=jar包位置 -DgroupId=设置groupId -DartifactId=设置artifactId -Dversion=设置版本 -Dpackaging=jar
```

示例：

```
mvn install:install-file -Dfile=D:\Maven\repository\com\google\zxing\core-3.4.1.jar -DgroupId=com.google.zxing -DartifactId=core -Dversion=3.4.1 -Dpackaging=jar
```

然后若IDEA中的依赖依然爆红，清空 IDEA 缓存，然后重启

![image-20230812172838966](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202308121728061.png)





##### 参考

[MAVEN将jar包添加到本地仓库](https://www.cnblogs.com/Marydon20170307/p/8883458.html)

[廖雪峰MAVEN介绍](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301146648610)





### 多模块项目的静态文件访问





##### 参考

[springboot 聚合项目 公共静态文件](https://juejin.cn/s/springboot%20%E8%81%9A%E5%90%88%E9%A1%B9%E7%9B%AE%20%E5%85%AC%E5%85%B1%E9%9D%99%E6%80%81%E6%96%87%E4%BB%B6)