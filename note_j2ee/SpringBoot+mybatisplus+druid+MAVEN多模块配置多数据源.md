# SpringBoot+MybatisPlus+Druid+MAVEN多模块项目 配置多数据源(Mysql Oracle)

*业务需求需要将之前项目写的某个模块集成到现在的项目中，需求很紧急，因为模块挪不过来的话，后续业务无法继续开发，第二天后端人员会处于空转，所以通宵查找各种教程完成了模块迁移，多数据源配置*。

我采用的解决方案是：MybatisPlus 多数据源配置，这种解决方案耦合度相对较低，并且配置简单可行度高。

> 其他解决方案：
>
> > 数据源配置类：
> >
> > ​		尝试了使用这个方法，找了很多教程试了很多遍，因为这种方法代码耦合度基本降到最低就可以实现多数据源配置，但是网上的所有教程都是关于单模块的没有涉及到多模块的教程，我们尝试配置看，在扫描类注解上使用通配符，但是mapper文件还是一直扫描不到，这个问题原因，应该是因为多模块时的扫描路径配置问题，目前还没有找到解决方案。
> >
> > 基于AOP的实现：
> >
> > ​		看到了几篇关于基于AOP的配置动态数据源的方法，但是代码耦合度太高了，在所有使用数据库的位置都要加注解，而且配置代价高，需要配置很多东西，最好不选择这种方案
>
> 综上：我们使用了 MybatisPlus的多数据源配置，我们只需要在使用从数据库的类上加上注解即可完成主从数据库切换，相对比较低耦合，且容易实现

### 目录结构

![image-20230816102701723](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202308161027798.png)

### 基本依赖

#### 以下依赖均在主 pom 文件中

SpringBoot

```
<parent>
    <artifactId>spring-boot-starter-parent</artifactId>
    <groupId>org.springframework.boot</groupId>
    <version>2.7.11</version>
</parent>
```

 原本的数据库：**mysql**

```
<dependency>
 	<groupId>mysql</groupId>
 	<artifactId>mysql-connector-java</artifactId>
 	<version>8.0.15</version>
</dependency>
```


新加的数据库：oracle

```
<dependency>
    	<groupId>com.oracle</groupId>
    	<artifactId>ojdbc6</artifactId>
    	<version>11.2.0.3</version>
</dependency>
```

数据库连接池：**druid**
```
<dependency>
   	<groupId>com.alibaba</groupId>
   	<artifactId>druid-spring-boot-starter</artifactId>
   	<version>1.1.21</version>
</dependency>
```

> **注意**druid一定要使用这个依赖，使用下面这个依赖会出问题（我也不知道为啥）

> ```
>  <dependency>
>    	<groupId>com.alibaba</groupId>
>    	<artifactId>druid</artifactId>
>    	<version>1.0.9</version>
>  </dependency>
> ```

**MybatisPlus**

```
<dependency>
   	<groupId>com.baomidou</groupId>
   	<artifactId>mybatis-plus-boot-starter</artifactId>
   	<version>3.5.3.2</version>
</dependency>
```

dynamic-datasource **多数据源**

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>3.6.1</version>
</dependency>
```



### 配置类

通过以下配置可以直接自动化配置为使用 druid 连接池的多数据源

```
spring:
# 提示：该配置为设置路径匹配方案，2.7x版本后路径匹配方式发生改变，需要手动设置回下面的版本
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher

  datasource:
    druid:
      initial-size: 5
      min-idle: 10
      max-active: 20
      # 配置获取连接等待超时的时间
      max-wait: 60000
      # 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位毫秒
      time-between-eviction-runs-millis: 60000
      # 配置一个连接在池中最小生存时间
      min-evictable-idle-time-millis: 300000
      validation-query: SELECT 1 FROM dual
      test-while-idle: true
      test-on-borrow: false
      test-on-return: false
      # 打开 PSCache，并且指定每个连接上 PSCache 的大小
      pool-prepared-statements: true
      max-pool-prepared-statement-per-connection-size: 20
      # 配置监控统计拦截的 Filter，去掉后监控界面 SQL 无法统计，wall 用于防火墙, 如果有日志，也需要过滤
      filters: stat,wall
      # 通过 connection-properties 属性打开 mergeSql 功能；慢 SQL 记录
      connection-properties: druid.stat.mergeSql\=true;druid.stat.slowSqlMillis\=5000
      # 配置 DruidStatFilter
      web-stat-filter:
        enabled: true
        url-pattern: /*
        exclusions: .js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico,/druid/*
      # 配置 DruidStatViewServlet
      stat-view-servlet:
        url-pattern: /druid/*
        # IP 白名单，没有配置或者为空，则允许所有访问
        # allow: 127.0.0.1
        # IP 黑名单，若白名单也存在，则优先使用
        # deny: 192.168.31.253
        # 禁用 HTML 中 Reset All 按钮
        reset-enable: false
        # 登录用户名/密码
        login-username: root
        login-password: 123456
        enabled: true
    dynamic:
      primary: master   #设置默认的数据源
      druid: #以下是支持的全局默认值，每个库还可重新设置独立参数
        # 初始连接数
        initialSize: 5
        # 最小连接池数量
        minIdle: 10
        # 最大连接池数量
        maxActive: 20
        # 配置获取连接等待超时的时间
        maxWait: 60000
        # 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
        timeBetweenEvictionRunsMillis: 60000
        # 配置一个连接在池中最小生存的时间，单位是毫秒
        minEvictableIdleTimeMillis: 300000
        # 配置一个连接在池中最大生存的时间，单位是毫秒
        maxEvictableIdleTimeMillis: 900000
        # 配置检测连接是否有效
        validationQuery: SELECT 1 FROM DUAL
        testWhileIdle: true
        testOnBorrow: false
        testOnReturn: false
      datasource:
        # mysql
        master:
          url: jdbc:mysql://10.1.40.87:3306/hebut_fresh_dev?serverTimezone=Hongkong&useUnicode=true&characterEncoding=utf-8&useSSL=false
          username: 6666
          password: 6666
          type: com.alibaba.druid.pool.DruidDataSource
          driver-class-name: com.mysql.cj.jdbc.Driver
        # oracle
        slave:
          url: jdbc:oracle:thin:@//10.1.40.88:1521/prod
          username: 66666
          # 初始化大小，最小，最大
          password: 66666
          driver-class-name: oracle.jdbc.OracleDriver
          initialSize: 5
          minIdle: 5
          maxActive: 20
          maxWait: 600000
          timeBetweenEvictionRunsMillis: 60000
          minEvictableIdleTimeMillis: 300000
          validation-query: SELECT 1 FROM DUAL
          testWhileIdle: true
          testOnBorrow: false
          testOnReturn: false
          filters: stat,slf4j #负载监控功能 此功能不支持hive
          dbType: oracle
```



### 使用方式

在需要使用副数据源的地方加上@DS注解即可，指定在配置文件中定义的副数据源名称即可

![image-20230816104823900](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202308161048006.png)

![image-20230816104839959](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202308161048017.png)



### 注意：

我在打包的时候遇到了，**程序包com.baomidou.dynamic.datasource.annotation不存在**的问题，本来以为是版本冲突，来回调试修改依赖版本，结果与版本无关。

最后解决办法：在使用了 @DS("slave") 注解的模块中也要加上下面依赖

**dynamic**-**datasource** **多数据源**

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>3.6.1</version>
</dependency>
```



##### 参考：

[【实战干货】Springboot实现多数据源整合的两种方式](https://juejin.cn/post/7025511138364227621)

[SpringBoot+MyBatisPlus+Druid从单数据源到多数据源动态切换3-基于dynamic-datasource实现多数据源](https://blog.csdn.net/gefeng1209/article/details/124673896)