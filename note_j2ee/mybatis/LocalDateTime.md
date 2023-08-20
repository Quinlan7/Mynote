# 使用mybatisplus时localDateTime不能映射问题解决

纯纯的版本问题，只需要升级druid，和mybatis的版本就可以

```
<!--druid-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.21</version>
</dependency>
<!--mybatis-plus的springboot支持，包含mybatis依赖-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3.1</version>
</dependency>
```