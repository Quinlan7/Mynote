# MAVEN在开发中遇到的问题

用于记录在开发中和maven有关的问题

### 一、当依赖源文件无jar包时疯狂报错

![image-20250225203323328](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/jdmac/20250225203323532.png)

1. 查询源文件夹发现目录下根本没有jar包，这属于一个 `POM`  类型的依赖，如果未指定 `<type>` 标签，Maven 默认认为依赖的类型是 `jar`。


![image-20250225203419835](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/jdmac/20250225203419902.png)

> - `pom` 类型的依赖通常用于管理依赖版本。
>
> - **支持非 JAR 依赖**：Maven 不仅支持 JAR 文件，还支持其他类型的文件（如 POM、WAR、EAR 等），`<type>` 标签用于处理这些非 JAR 依赖。
> - 如果未指定 `<type>` 标签，Maven 默认认为依赖的类型是 `jar`。

2. 修改依赖类型，添加 `type` 标签，属性为 POM

```xml
<dependency>
            <groupId>com.jd.laf.config</groupId>
            <artifactId>laf-config-client-jd-spring</artifactId>
            <version>1.4.1</version>
            <type>pom</type>
</dependency>
```

3. 解决报错

![image-20250225204154673](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/jdmac/20250226104901850.png)