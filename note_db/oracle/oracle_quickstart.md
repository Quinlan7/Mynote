# quickstart

*开发中遇到的*

### 分析聚合函数

```sql
SELECT STUDENT_NAME,STUDENT_CODE,MAJOR_LABEL,CLASS_LABEL,OVERALL_GRADE,row_number() OVER(partition by MAJOR_LABEL order by OVERALL_GRADE DESC) as grade
FROM "EXTRACURRICULAR_SCORE_SUMMARY" 

```

> 分析函数
>
> + row_number()
> + rank()
> + dense_rank()

##### 参考

[[Oracle学习笔记：窗口函数]](https://www.cnblogs.com/hider/p/11678804.html)

### 截取数字前几位

```sql
SUBSTR(STUDENT_CODE, 1, 2)
```





### 字符串中有单引号要怎么办

如果您的表名包含单引号，您可以使用两个单引号来转义单引号字符。在SQL中，两个单引号''表示一个单引号字符。以下是在查询中处理包含单引号的表名的方法：

```sql
SELECT 
  segment_name AS table_name,
  segment_type AS segment_type,
  bytes / (1024 * 1024) AS size_mb
FROM dba_segments
WHERE segment_name = '<table_name_with_single_quote''s>';
```

在上述示例中，`<table_name_with_single_quote''s>` 是您的表名，其中两个单引号''表示一个单引号字符。这将允许您在包含单引号的表名的情况下运行查询。

请确保在单引号字符之前和之后都使用两个单引号来正确转义表名中的单引号，以避免语法错误。



### with as

> SQL中的`WITH`语法通常用于创建临时的命名查询结果集，也被称为"公共表表达式"（Common Table Expression，CTE）。`WITH`语句使您能够编写子查询，并为其命名，然后在主查询中引用这些子查询，以提高可读性和维护性。通常，`WITH`子句位于SQL查询的开头。
>
> `WITH`语法的一般形式如下：
>
> ```sql
> WITH cte_name (column1, column2, ...) AS (
>     -- CTE查询
>     SELECT ...
>     FROM ...
>     WHERE ...
> )
> -- 主查询
> SELECT ...
> FROM cte_name
> WHERE ...
> ```
>
> 这里是`WITH`语法的关键组成部分：
>
> - `cte_name`: 这是您为临时查询结果集指定的名称，您可以在主查询中引用它。这是您自定义的名称，用于标识CTE。
> - `(column1, column2, ...)`: 可选部分，允许您为CTE的列指定列名，从而命名结果集的列。
> - `AS`: 用于将CTE的定义与名称分开。
> - `-- CTE查询`: 这是实际的子查询，它可以是任何有效的SQL查询，包括`SELECT`语句，`JOIN`操作，聚合等。CTE的结果集将在主查询中使用。
> - 主查询：在`WITH`子句之后，您可以编写主查询，可以引用前面定义的CTE。在主查询中，您可以使用CTE的名称，就像它是一个表一样，以访问其结果集。
>
> 以下是一个示例，演示如何使用`WITH`语法创建CTE：
>
> ```sql
> WITH EmployeeCTE AS (
>     SELECT EmployeeID, FirstName, LastName
>     FROM Employees
>     WHERE Department = 'IT'
> )
> SELECT EmployeeID, FirstName, LastName
> FROM EmployeeCTE
> WHERE Salary > 60000;
> ```
>
> 在上面的示例中，我们首先定义了一个名为`EmployeeCTE`的CTE，它选择了IT部门的员工。然后，在主查询中，我们使用了`EmployeeCTE`来选择在IT部门工作且薪水超过60000的员工。这使查询更具可读性和组织性。



### oracle 大小写

+ oracle建表名和列名都要大写

> oracle sql 执行时默认所有单词都是大写，要保留小的的话，用双引号扩起来
