#### 11.15

1. （4）折线图接口

   前端参数：accountId、roleId、id（比如若是在专业维度，id就是具体的哪个专业编码；若是在班级维度，id就是具体的班级编码）、dimensionId（代表维度：专业维度、班级维度、宿舍维度、老师维度）

   后端返回：List<Map<String, Object>>

   每一项的参数为time、number、intentionState，其中time是时间以星期或者天数（后端哪个方便选哪个）(天数)为最小单位，number是人数，intentionState是意向名（拼接，例：）

    

   intentionState在Dao层只返回Intention_name然后再service拼接，因为只需要最终状态的人数，

   SQL  返回   ~~updatetime~~    number  intentionState

   + sql限制条件：
     + stulist中
     + 每个intentionid所对应的都应该是最终状态
     + 在开始时间和截至时间内
     + 对应的维度的对应的维度ID

+ 因为时间由后端来生成（在service层），所以数据库不返回时间了，在service层增加时间点。每次查询一个星期内的人数，返回的时间为==从当天开始每隔七天的yyyy-MM-dd==
+ 在后端生成当前的时间（DATE：yyyy-MM-dd HH:mm:ss）

##### 难点

我们在数据库查询的时候只想要yyyy-mm-dd，但是DATE有HH：mm：ss如何映射到xml里让它只有yyyy-mm-dd

+ 使用指定的jdbc-type：date（试试）



```
AND S.UPDATE_TIME &lt; "TO_DATE" (#{timePoint},'yyyy-mm-dd hh24:mi:ss')
```

##

