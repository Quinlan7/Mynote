# 牛客

<br>

### SQL22

##### 需求：

运营想要了解每个学校答过题的用户平均答题数量情况，请你取出数据。

##### 示例：

![捕获](SQL_niuke.assets/捕获.JPG)

![捕获啊啊](SQL_niuke.assets/捕获啊啊.JPG)

##### SQL：

```sql
SELECT
  university,
  COUNT(question_id)/COUNT(DISTINCT u.device_id)  avg_answer_cnt
FROM
  user_profile u
  INNER JOIN question_practice_detail q 
  ON u.device_id = q.device_id
GROUP BY
  u.university
ORDER BY
  u.university;
```

==注意==:COUNT() 函数中也可以用DISTINCT，来统计不同的值的数量

<br>

### SQL26

##### 需求：

现在运营想要将用户划分为25岁以下和25岁及以上两个年龄段，分别查看这两个年龄段用户数量

**本题注意：age为null 也记为 25岁以下**

##### 示例：

![image-20230309084437576](SQL_niuke.assets/image-20230309084437576.png)

![image-20230309084454154](SQL_niuke.assets/image-20230309084454154.png)

##### SQL：

```sql
SELECT IF(age >= 25 ,'25岁及以上','25岁以下' ) age_cut ,COUNT(*) number
FROM user_profile
group by age_cut
```

##### 知识点：

对于SQL的三个条件函数的使用，并且是可以对条件函数进行分组的！

<br>

### SQL28

+ 错误的：原因是mysql底层对于sql执行顺序有自己的优化，所以会先执行select后执行group by，所以执行到select中的day时，还没有执行group by，报错

```sql
select  day,count(*) question_cnt
from question_practice_detail
where YEAR(date) = 2021 and MONTH(date) = 08
group by DAY(date) day

```

+ 正确的

```sql
select  DAY(date) day,count(*) question_cnt
from question_practice_detail
where YEAR(date) = 2021 and MONTH(date) = 08
group by DAY(date)

```

<br>

### SQL29

##### 需求：

现在运营想要查看用户在某天刷题后第二天还会再来刷题的平均概率。请你取出相应数据。

##### 示例：

![image-20230312112954184](SQL_niuke.assets/image-20230312112954184.png)

![image-20230312113102837](SQL_niuke.assets/image-20230312113102837.png)

##### 理解：

用户在某天刷题后第二天还会再来刷题的平均概率.

既然概率则确定分子分母即可：

分母：这个有个坑，即如果一个用户在同一天刷了不同的题很多次，则会有同一用户在同一天的很多条记录，这个情况在分母中是需要排除的，分母排除掉用户编号和日期都相同的记录，在使用这个表的时候其实每次用的都是排除之后的数据，在分子中也一样，我们称为纯表。

分子：在纯表中的所有日期数据加一nextday，然后和没有加一的纯表对照，用户编号和nextday与day相等的数据筛选出来即可。



##### 解答：

```mysql
select
    (
        select
            count(*)
        from
            (
                select distinct
                    device_id,
                    date
                from
                    question_practice_detail
            ) A
            inner join (
                select distinct
                    device_id,
                    date_add (date, interval 1 day) nextday
                from
                    question_practice_detail
            ) B on A.date = B.nextday
            and A.device_id = B.device_id
    ) / count(distinct device_id, date) avg_ret
from
    question_practice_detail

```

<br>

### SQL34

##### 需求：

题目： 现在运营想要了解复旦大学的每个用户在8月份练习的总题目数和回答正确的题目数情况，请取出相应明细数据，对于在8月份没有练习过的用户，答题数结果返回0.

##### 示例：

<img src="SQL_niuke.assets/image-20230316114538151.png" alt="image-2023031614538151" style="zoom:50%;" />

<img src="SQL_niuke.assets/image-20230316114544928.png" alt="image-2023036114544928" style="zoom:50%;" />

##### 结果示例

<img src="SQL_niuke.assets/image-20230316114557942.png" alt="image-20230316114557942" style="zoom:50%;" />

##### sql:

对于聚合函数和条件函数的复合使用

```sql
select
    u.device_id,
    u.university,
    # if(count(*) != 0,count(*),0 ) 
    sum(if(month(q.date) = 08,1,0)) question_cnt,
    sum(if(q.result = 'right',1,0)) right_question_cnt
from
    user_profile u
    left join question_practice_detail q on u.device_id = q.device_id
where
    u.university = '复旦大学'
#    and month (q.date) = 08
group by
    u.device_id

```

