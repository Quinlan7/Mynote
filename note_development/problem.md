# 遇到的问题

### 邮箱验证码

邮箱池

### 前后端跨域时的cookie





### Double和double的精度问题

建议直接用BigDeciamal，并且在数据库映射的时候oracle的number直接映射BigDecimal



### 多模块打包问题

将下面这段代码存放在你的启动模块的pom中，并且删除掉非父模块的其他模块的maven的plugin

```xml
<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>${spring-boot.version}</version>
				<configuration>
					<fork>true</fork> <!-- 如果没有该配置，devtools不会生效 -->
				</configuration>
				<executions>
					<execution>
						<goals>
							<goal>repackage</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
```

