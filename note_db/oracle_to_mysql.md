# oracle to mysql

*项目需求：底层数据库由Oracl改为Mysql！*

### 语法区别

> MySQL和Oracle是两种不同的关系型数据库管理系统，它们使用的SQL语句在一些方面存在一些区别。以下是一些可能的MySQL和Oracle数据库之间的SQL语句区别：
>
> 1. 数据类型：MySQL和Oracle可能具有不同的数据类型。例如，在MySQL中，使用TINYINT来表示布尔类型，而在Oracle中，使用NUMBER(1)或CHAR(1)来表示布尔类型。
>
> 2. 字符串处理：在MySQL中，使用单引号(' ')表示字符串字面值，而在Oracle中，使用单引号(' ')或双引号(" ")都可以表示字符串字面值。此外，Oracle还支持使用N前缀来表示Unicode字符串，如N'Hello'。
>
> 3. 日期和时间处理：MySQL和Oracle在处理日期和时间类型的方式上可能有所不同。例如，在MySQL中，使用DATE、DATETIME和TIMESTAMP等数据类型来表示日期和时间，而在Oracle中，使用DATE、TIMESTAMP和INTERVAL等数据类型来表示日期和时间。
>
> 4. 自增字段：在MySQL中，可以使用AUTO_INCREMENT属性来创建自增字段，而在Oracle中，可以使用序列（Sequence）和触发器（Trigger）来实现自增字段的功能。
>
> 5. 分页查询：在MySQL中，使用LIMIT关键字来进行分页查询，如LIMIT 0, 10表示从第0行开始获取10行记录。而在Oracle中，使用ROWNUM或者ROW_NUMBER() OVER函数来实现类似的分页功能。
>
> 6. NULL值处理：在MySQL中，NULL被视为一个特殊的值，可以直接与其他值进行比较。而在Oracle中，NULL被视为未知的值，需要使用IS NULL或IS NOT NULL进行比较。
>
> 7. 存储过程和触发器：MySQL和Oracle在存储过程和触发器的语法和语义上有很大的区别。例如，在MySQL中，使用BEGIN和END来定义存储过程的代码块，而在Oracle中，使用DECLARE、BEGIN和END来定义存储过程的代码块。
>
> 这只是一些可能的MySQL和Oracle数据库之间SQL语句的区别，并不是详尽无遗。具体的差异还可能取决于使用的版本、配置和设置等因素。在编写跨不同数据库系统的SQL语句时，应该仔细了解各个数据库系统的文档和规范，并进行相应的调整和测试。



+ mysql关键字
  + rank在mysql中是关键字会报错   ，  检索出的列名是关键字，==加单引号==
  + condition在mysql中是关键字  ，表名是关键字，==加反引号==
  + 
+ oracle 中NVL函数

  + nvl(t.avg_score, 0.0)
    
  + COALESCE(t.avg_score, 0.0)
  
+ to_char(entry_time, 'yyyy-MM-dd') 函数

  + DATE_FORMAT(entry_time, '%Y-%m-%d')


+ 注意在xml右键执行sql测试的时候
  + 在foreach语句中的参数要加括号
  + 如果有mybatis中的\<where>标签会报错（与sql无关）

+ oracle 别名查询出来自动大写（如果不加单引号，加上单引号才可以小写）
  + mysql查出来别名大写就大写，小写就小写

+ oracle中的to_char(to_number(#{warnYear})+1)怎么改成mysql

  + > 在将 Oracle SQL 中的 `to_char(to_number(#{warnYear})+1)` 转换为 MySQL 语句时，可以使用 MySQL 的 `CONVERT` 函数来实现类似的功能。下面是将该表达式转换为 MySQL 语句的示例：
    >
    > ```sql
    > CONVERT((CAST(#{warnYear} AS UNSIGNED) + 1), CHAR)
    > ```
    >
    > 在 MySQL 中，`CAST` 函数可以用来将字符串转换为数字型，而 `UNSIGNED` 可以用来指定无符号整数类型。然后，再使用 `CONVERT` 函数将计算结果转换为字符型，从而实现类似 Oracle 中 `to_char` 函数的功能。
    >
    > 需要注意的是，具体的语法和函数使用可能因 MySQL 版本而异，建议根据实际情况进行调整。同时，还应该注意检查转换后的 MySQL 语句是否符合实际需求，包括数据类型和计算逻辑等方面。

  + mysql select * 的列名是大写的！

