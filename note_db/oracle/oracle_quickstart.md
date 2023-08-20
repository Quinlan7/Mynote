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







### with as

