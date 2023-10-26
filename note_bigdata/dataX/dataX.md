# DataX



## 一、setting

### errorLimit

> "errorLimit" 字段用于指定 DataX 任务的错误限制，以控制任务的行为和终止条件。它可以包含以下属性值：
>
> 1. "record"：这是最常见的错误限制属性。它指定允许的最大错误记录数。如果任务处理的错误记录数超过这个限制，任务将终止。例如，设置 "errorLimit" 的 "record" 属性为 0 表示不允许任何错误记录，而设置为 100 表示允许最多 100 条错误记录。
>
> 2. "percentage"：这个属性指定允许的最大错误记录占总记录数的百分比。如果任务中错误记录的比例超过这个百分比，任务将终止。例如，设置 "errorLimit" 的 "percentage" 属性为 10 表示允许错误记录占总记录数的最多 10%。
>
> 3. "record" 和 "percentage" 的组合：你还可以同时指定 "record" 和 "percentage"，以便在达到其中一个限制时终止任务。任务将在两者中的任何一个条件被满足时终止。
>
> 以下是一个示例 JSON 配置中 "errorLimit" 的使用示例：
>
> ```json
> {
>   "job": {
>     "setting": {
>       "errorLimit": {
>         "record": 100,     // 最多允许 100 条错误记录
>         "percentage": 5    // 错误记录不得超过总记录数的 5%
>       }
>     },
>     // 其他任务配置
>   }
> }
> ```
>
> 在上述示例中，任务允许最多 100 条错误记录或错误记录不得超过总记录数的 5%。当其中一个条件被满足时，任务将终止。你可以根据任务的需求来调整这些限制属性的值，以控制任务的错误处理行为。







## 二、reader



### splitPk

+ 用于 datax 从数据源抽取数据时加速

> "splitPk" 是 DataX 任务配置中的一个字段，用于指定用于数据分片的列（Split Primary Key）。这个字段的作用是告诉 DataX 如何将数据进行分片处理，以便并行地抽取、转换和加载数据。
>
> 在关系型数据库中，通常会有一个主键（Primary Key），它是唯一标识每个记录的字段。"splitPk" 字段允许你指定一个主键列，DataX 将使用这个主键列的值来划分数据并同时处理多个分片。这有助于加速数据传输和处理过程，特别是在大规模数据迁移或数据同步操作中，可以更高效地处理数据。
>
> 以下是一个示例 JSON 配置中的 "splitPk" 字段：
>
> ```json
> {
>   "job": {
>     "content": [
>       {
>         "reader": {
>           "name": "mysqlreader",
>           "parameter": {
>             "connection": [
>               {
>                 "jdbcUrl": "jdbc:mysql://localhost:3306/mydb",
>                 "table": ["mytable"],
>                 "username": "username",
>                 "password": "password"
>               }
>             ],
>             "splitPk": "id"  // 这里指定了主键列为 "id"
>           }
>         },
>         "writer": {
>           "name": "mysqlwriter",
>           "parameter": {
>             "connection": [
>               {
>                 "jdbcUrl": "jdbc:mysql://localhost:3306/mydb",
>                 "table": ["mytargettable"],
>                 "username": "username",
>                 "password": "password"
>               }
>             ]
>           }
>         }
>       }
>     ]
>   }
> }
> ```
>
> 在上述示例中，"splitPk" 被设置为 "id"，这意味着 DataX 将使用 "id" 列的值来分片数据并进行处理。这对于从 MySQL 数据库中抽取数据并将其加载到目标数据库中非常有用。
>
> 总之，"splitPk" 字段有助于提高 DataX 任务的性能和效率，特别是在处理大量数据时，通过并行分片处理，可以更快地完成数据迁移和同步操作。

### **where**

+ Reader根据指定的column、table、where条件拼接SQL

### querySql

> 描述：在有些业务场景下，where这一配置项不足以描述所筛选的条件，用户可以通过该配置型来自定义筛选SQL。当用户配置了这一项之后，DataX系统就会忽略table，column这些配置型，直接使用这个配置项的内容对数据进行筛选，例如需要进行多表join后同步数据，使用select a,b from table_a join table_b on table_a.id = table_b.id
>
> **注意**：当用户配置querySql时，MysqlReader直接忽略table、column、where条件的配置，querySql优先级大于table、column、where选项。





## 三、writer



### preSql

- 描述：写入数据到目的表前，会先执行这里的标准语句。如果Sql中有你需要操作到的表名称，请使用**@table**表示，这样在实际执行Sql语句时，会对变量按照实际表名称进行替换。比如你的任务是要写入到目的端的100个同构分表（表名称为:datax_00,datax01, … datax_98,datax_99），并且你希望导入数据前，先对表中数据进行删除操作，那么你可以这样配置：”preSql”:[“delete from 表名”]，效果是：在执行到每个表写入数据前，会先执行对应的delete from对应表名称
- 必选：否
- 默认值：无

### postSql

- 描述：写入数据到目的表后，会执行这里的标准语句。（原理同 preSql 。）
- 必选：否
- 默认值：无

### writeMode

- 描述：控制写入数据到目标表采用**insert into**或者**replace into**或者 **ON DUPLICATE KEY UPDATE**语句
- 必选：是
- 所有选项：insert/replace/update
- 默认值：insert

### batchSize

- 描述：一次性批量提交的记录数大小，该值可以极大减少DataX与Mysql的**网络交互次数**，并提升整体**吞吐量**。但是该值设置过大可能会造成DataX运行进程**OOM**情况。
- 必选：否
- 默认值：1024





## 四、DataX-Web



- [增量参数设置](https://github.com/WeiYe-Jing/datax-web/blob/master/doc/datax-web/increment-desc.md)
- [分区参数设置](https://github.com/WeiYe-Jing/datax-web/blob/master/doc/datax-web/partition-dynamic-param.md)











### 参考

[树懒学堂](https://www.shulanxt.com/datawarehouse/datax/dataxmysqlreader)

[datax-web](https://github.com/WeiYe-Jing/datax-web)