# oracle如何实现关联表批量插入

[oracle with as介绍](https://www.cnblogs.com/mingforyou/p/8295239.html)

[深入理解和使用Oracle中with as语句以及与增删改查的结合使用](https://blog.csdn.net/baidu_37107022/article/details/79619809)

[Oracle Dual 表详解](https://blog.csdn.net/tianlesoftware/article/details/4764326)

[SQL中TRUNC函数的用法](https://blog.csdn.net/Schaffer_W/article/details/109046478)

[oracle随机函数dbms_random](https://www.cnblogs.com/bolang100/p/7158703.html)

[Oracle数据库的包概念](https://blog.csdn.net/shangyang326/article/details/3299501)

[Oracle常用包](https://juejin.cn/post/6997682048429359140 )

 

--插入总意向表

insert into EE_STU.GRA_INTENTION_STU 

select

 \* from

 

(

WITH tablea AS ( SELECT USERID as STU_ID, ENROLLYEAR as ENROLL_YEAR, MAJORID as MAJOR_ID FROM EE_STU.STUDENTPROFILE WHERE ENROLLYEAR = '2020' ),

tableb AS ( SELECT trunc( dbms_random.value ( 1, 9 ) ) AS INTENTION_ID FROM dual ),

tablec AS ( SELECT fun1 ( ) AS INTENTION_CITY FROM dual ),

tabled AS ( SELECT trunc( dbms_random.value ( 1, 5 ) ) AS INTENTION_SALARY FROM dual ),

tablee AS ( SELECT trunc( dbms_random.value ( 1, 9 ) ) AS INTENTION_SEC_ID FROM dual )

SELECT

STU_ID,INTENTION_ID,INTENTION_CITY,INTENTION_SALARY,INTENTION_SEC_ID,MAJOR_ID,ENROLL_YEAR,TO_DATE('2022-10-05 14:13:57','yyyy-MM-dd HH24:mi:ss') as UPDATE_TIME

FROM

​      tablea,

​      tableb,

​      tablec,

​      tabled,

​      tablee

) 

 

--删除所有数据

 delete from EE_STU.GRA_INTENTION_STU

 

 

--创建随机抽取城市id函数

create or replace function fun1

return varchar2 authid current_user as

str varchar2(100);

stmt varchar2(2000);

begin

stmt := 'SELECT

​           CITYID 

​      FROM

​           ( SELECT CITYID FROM EE_STU.CITY ORDER BY dbms_random.value ) 

​      WHERE

​           ROWNUM < 2';

 

execute immediate stmt

  into str;

return str;

END;

 

--插入无意向学生关联表

insert into EE_STU.GRA_INTENTION_NONE

select * from 

(with t1 as (select STU_ID,UPDATE_TIME from 

EE_STU.GRA_INTENTION_STU

WHERE INTENTION_ID = '8'),

t2 AS ( SELECT trunc( dbms_random.value ( 1, 6 ) ) AS REASON FROM dual )

 

select 

STU_ID,REASON,UPDATE_TIME

from

t1,t2

)

 

--考公关联表插入

 delete from EE_STU.GRA_INTENTION_GOLDBOWL

 

insert into EE_STU.GRA_INTENTION_GOLDBOWL

select * from 

(with t1 as (select STU_ID,UPDATE_TIME from 

EE_STU.GRA_INTENTION_STU

WHERE INTENTION_ID = '3'),

t2 AS ( SELECT trunc( dbms_random.value ( 1, 6 ) ) AS STATE FROM dual )

select 

STU_ID,STATE,UPDATE_TIME

from

t1,t2

)

 

--留学相关

 delete from EE_STU.GRA_INTENTION_ABROAD

 

insert into EE_STU.GRA_INTENTION_ABROAD

(

SELECT

​      STU_ID,

​      b.COUNTRY AS COUNTRY,

​      a.SCHOOL as SCHOOL ,

​      STATE,

​      UPDATE_TIME

FROM

​      (

​           WITH t1 AS ( SELECT STU_ID, UPDATE_TIME FROM EE_STU.GRA_INTENTION_STU WHERE INTENTION_ID = '2' ),

​           t2 AS ( SELECT trunc( dbms_random.value ( 0, 6 ) ) AS STATE FROM dual ),

​           t3 AS ( SELECT fun2 ( ) AS SCHOOL FROM dual ) SELECT

​           STU_ID,

​           STATE,

​           UPDATE_TIME,

​           SCHOOL 

​      FROM

​           t1,

​           t2,

​           t3 

​      ) a

​      LEFT JOIN EE_STU.COUNTRY_SCHOOL b ON a.SCHOOL = b.SCHOOL

)

 

--返回随机一个外国大学的函数

create or replace function fun2

return varchar2 authid current_user as

str varchar2(100);

stmt varchar2(2000);

begin

stmt := 'SELECT

​           SCHOOL

​      FROM

​           ( SELECT * FROM EE_STU.COUNTRY_SCHOOL ORDER BY dbms_random.value ) 

​      WHERE

​           ROWNUM < 2';

 

execute immediate stmt

  into str;

return str;

END;

 

--考研相关

delete from EE_STU.GRA_INTENTION_POSTGRA

 

 

insert into EE_STU.GRA_INTENTION_POSTGRA

(

SELECT

​      STU_ID,

​      a.SCHOOL as SCHOOL ,

​      MAJOR,

​      b.TYPE as SCHOOL_TYPE,

​      STATE,

​      UPDATE_TIME

FROM

​      (

​           WITH t1 AS ( SELECT STU_ID, UPDATE_TIME FROM EE_STU.GRA_INTENTION_STU WHERE INTENTION_ID = '5' ),

​           t2 AS ( SELECT trunc( dbms_random.value ( 1, 4 ) ) AS STATE FROM dual ),

​           t3 AS ( SELECT fun2 ( ) AS SCHOOL FROM dual ),

​           t4 AS ( SELECT fun3 ( ) AS MAJOR      FROM dual )

​           SELECT

​           STU_ID,

​           STATE,

​           UPDATE_TIME,

​           SCHOOL ,

​           MAJOR

​      FROM

​           t1,

​           t2,

​           t3 ,

​           t4

​      ) a

​      LEFT JOIN EE_STU.SCHOOL_CHINA b ON a.SCHOOL = b.SCHOOL

)

 

 

--返回随机一个国内大学的函数

create or replace function fun2

return varchar2 authid current_user as

str varchar2(100);

stmt varchar2(2000);

begin

stmt := 'SELECT

​           SCHOOL

​      FROM

​           ( SELECT * FROM EE_STU.SCHOOL_CHINA ORDER BY dbms_random.value ) 

​      WHERE

​           ROWNUM < 2';

 

execute immediate stmt

  into str;

return str;

END;

 

--随机回一个专业

 

create or replace function fun3

return varchar2 authid current_user as

str varchar2(100);

stmt varchar2(2000);

begin

stmt := 'SELECT

​           MAJOR

​      FROM

​           ( SELECT * FROM EE_STU.MAJOR_TEMP ORDER BY dbms_random.value ) 

​      WHERE

​           ROWNUM < 2';

 

execute immediate stmt

  into str;

return str;

END;

 

 

最新时间

SELECT

​      \* 

FROM

​      (

​      SELECT

​           a.STU_ID,

​           a.UPDATE_TIME,

​           row_number ( ) over ( partition BY a.STU_ID ORDER BY a.UPDATE_TIME DESC) AS rn 

​      FROM

​           EE_STU.GRA_INTENTION_STU a 

​           where STU_ID = '200625'

​       

​      ) 

WHERE

​      rn =1