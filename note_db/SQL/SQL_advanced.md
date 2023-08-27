# 视图、存储过程、触发器、游标



## 一、视图

+ 视图会不会自动更新

> 是的，MySQL 中的视图会自动更新。视图是虚拟表，其内容是根据其定义的基础表或其他视图的查询结果动态生成的。当你查询视图时，MySQL 会实时计算和生成视图的内容，以反映基础表中的最新数据。
>
> 无论是插入、更新还是删除基础表的数据，当你查询视图时，它都会自动反映这些变化。这是 MySQL 视图的一个关键特性，它使得视图成为一个方便的工具，用于聚合、过滤和处理数据，而无需手动执行操作来保持视图内容的最新状态。
>
> 所以，你不需要手动更新视图，因为视图会在查询时根据基础表的数据动态生成和更新内容。

数据库视图（Database View）是一个虚拟表，它是基于一个或多个基础表（或其他视图）的查询结果生成的结果集。视图并不实际存储数据，而是根据定义的查询规则在查询时动态生成数据。视图允许你以一种逻辑方式查看、过滤和操作数据，从而简化复杂的查询操作和数据操作。

## 二、存储过程

## 三、触发器

触发器相对其实比较简单，只需要记住触发器的三要素就可以理解触发器了。

+ 事件类型：增删改，即`insert`、`delete`和`update`；
+ 触发时间：事件类型前和后，即`before`和`after`；
+ 触发对象：表中的每一条记录（行），即`整张表`。

```mysql
DELIMITER //
CREATE TRIGGER trg_delete_student_update_view
AFTER DELETE ON students FOR EACH ROW
BEGIN
    DELETE FROM character_stu_year_all WHERE id = OLD.id;
END;
//
DELIMITER ;
```

关于DELIMITER：

> 在 MySQL 中，`DELIMITER` 是一个命令，用于更改 SQL 语句中的语句分隔符。默认情况下，MySQL 使用分号 `;` 作为语句分隔符，以便一次性执行多个 SQL 语句。然而，有些情况下，你可能需要在一个 SQL 语句中使用分号，比如在存储过程或触发器的定义中。
>
> 使用 `DELIMITER` 命令，你可以更改语句分隔符为其他字符，以避免在 SQL 语句中的特定部分使用分号时引发误解。
>
> 以下是 `DELIMITER` 命令的基本语法：
>
> ```sql
> DELIMITER new_delimiter;
> ```
>
> 其中，`new_delimiter` 是你想要设置的新的语句分隔符。一旦设置了新的分隔符，你可以在 SQL 脚本中使用这个新的分隔符来分隔不同的语句。
>
> 例如，如果你要定义一个存储过程，其中包含多个语句，并且其中的某个语句使用了分号，你可以使用 `DELIMITER` 命令来更改分隔符：
>
> ```sql
> DELIMITER //
> CREATE PROCEDURE example_procedure()
> BEGIN
>     -- 这里可以使用分号
>     SELECT 'Hello, World!';
> END;
> //
> DELIMITER ;
> ```
>
> 在上面的例子中，通过将 `DELIMITER` 设置为 `//`，我们使得存储过程的定义使用了 `//` 作为分隔符，以避免与语句内的分号产生冲突。
>
> 一旦你完成了需要使用新分隔符的部分，记得通过 `DELIMITER ;` 将分隔符恢复为默认的分号。

[史上最简单的 MySQL 教程（三十）「触发器」](https://github.com/guobinhit/mysql-tutorial/blob/master/articles/trigger.md)

[MySQL触发器trigger的使用](https://www.cnblogs.com/geaozhang/p/6819648.html)

## 四、游标

## 五、函数





##### 参考

[【MySQL进阶教程】视图/存储过程/触发器](https://juejin.cn/post/7204121228017467453)