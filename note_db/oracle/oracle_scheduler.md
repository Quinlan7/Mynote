# Oracle scheduler JOB 定时任务



## 定时任务

job是oracle的定时任务，又叫定时器，定时作业，作业定时地自动执行一些脚本，或作数据备份，或作数据提炼，或作数据库性能的优化，或作重建索引等等的工作，需要用到job。

Job是一种被调度执行的任务。Job可以是一个PL/SQL块、一个SQL语句、一个外部脚本或程序等。它们可以被定时调度执行，也可以被手动启动执行。

 job定时任务还有另一种实现方式 scheduler，推荐后者，因为使用dbms_job提交的job在Oracle 10g及以上版本中已经被废弃，推荐使用**dbms_scheduler**包提交job。



## 基础语法



### 创建job

```sql
BEGIN
  DBMS_SCHEDULER.CREATE_JOB (
    job_name        => 'job_name',           -- job的名称
    job_type        => 'PLSQL_BLOCK',        -- job的类型，可以是PLSQL_BLOCK、STORED_PROCEDURE等
    job_action      => 'begin my_proc(); end;',  -- job执行的脚本或存储过程
    start_date      => SYSTIMESTAMP,         -- job开始执行的时间
    repeat_interval => 'FREQ=DAILY; INTERVAL=1',  -- job执行的间隔时间
    enabled         => TRUE                  -- 是否启用job
  );
END;
```



###  **查看Job运行情况**

```sql
SELECT job_name, job_type, enabled, state, last_start_date, next_run_date
FROM dba_scheduler_jobs;


SELECT *
FROM dba_scheduler_jobs
WHERE job_name = 'JOB_NAME';

SELECT job_name, state, last_start_date, next_run_date,enabled
FROM dba_scheduler_jobs
WHERE job_name = 'JOB_NAME';
 
```

### JOB 停止

```sql
BEGIN
  DBMS_SCHEDULER.STOP_JOB (
    job_name        => 'job_name',
    force_option    => 'IMMEDIATE',
    commit_semantics=> 'ABORT');
END;
```



### JOB启动和删除

启动

```sql
BEGIN
  DBMS_SCHEDULER.RUN_JOB (
    job_name        => 'JOB_NAME',
    use_current_session => FALSE);
END;
```

删除

```sql
BEGIN
  DBMS_SCHEDULER.DROP_JOB (
    job_name        => 'CLEAN_REALTIME_ETCEXIT_DATA_JOB',           -- job的名称
    force           => FALSE                 -- 是否强制删除job
  );
END;
```





### 定时表达式

```
--每天运行一次
    'SYSDATE + 1'
 
--每小时运行一次
    'SYSDATE + 1/24'
 
--每10分钟运行一次                 
    'SYSDATE + 10/（60*24）'
 
--每30秒运行一次                    
    'SYSDATE + 30/(60*24*60)'
 
--每隔一星期运行一次               
    'SYSDATE + 7'
 
--每个月最后一天运行一次          
    'TRUNC(LAST_DAY(ADD_MONTHS(SYSDATE,1))) + 23/24'
 
--每年1月1号零时                    
    'TRUNC(LAST_DAY(TO_DATE(EXTRACT(YEAR FROM SYSDATE)||'12'||'01','YYYY-MM-DD'))+1)'
 
--每天午夜12点                       
    'TRUNC(SYSDATE + 1)'
 
--每天早上8点30分                  
    'TRUNC(SYSDATE + 1) + (8*60+30)/(24*60)'
 
--每星期二中午12点                 
    'NEXT_DAY(TRUNC(SYSDATE ), ''TUESDAY'' ) + 12/24'
 
--每个月第一天的午夜12点        
    'TRUNC(LAST_DAY(SYSDATE ) + 1)'
 
--每个月最后一天的23点           
    'TRUNC (LAST_DAY (SYSDATE)) + 23 / 24'
 
--每个季度最后一天的晚上11点  
    'TRUNC(ADD_MONTHS(SYSDATE + 2/24, 3 ), 'Q' ) -1/24'
 
--每星期六和日早上6点10分      
    'TRUNC(LEAST(NEXT_DAY(SYSDATE, ''SATURDAY"), NEXT_DAY(SYSDATE, "SUNDAY"))) + (6*60+10)/(24*60)'
```



## 实战



### 创建存储过程

```sql
create or replace procedure REALTIME_ETCEXIT_DELETE as
begin
  DELETE FROM "REALTIME_ETCTS_EXIT";
  commit;
end;
```



### 创建 JOB

```sql
BEGIN
  DBMS_SCHEDULER.CREATE_JOB (
    job_name        => 'clean_realtime_etcexit_data_job',
    job_type        => 'STORED_PROCEDURE',
    job_action      => 'REALTIME_ETCEXIT_DELETE',
    start_date      => SYSDATE,
    repeat_interval => 'FREQ=DAILY; BYHOUR=0; BYMINUTE=0; BYSECOND=0',
    enabled         => TRUE,
    comments        => '清理过期数据');
END;
```





## 踩坑

*oracle系统时间，会话时间,可能和你的本地时间不一样*

> 实战发现影响定时任务时间的是会话时区，如果你使用的是 DataGrip 连接数据库，你可以在配置连接属性时设置 时区

![image-20231103165926425](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311031659641.png)

##### 查看数据库系统时区

```sql
SELECT DBTIMEZONE FROM DUAL;
```

##### 查看会话时区

```
select sessiontimezone from dual;
```

##### 修改数据库时区

```sql
alter database set time_zone='+8:00';
```

然后重启数据库

##### 修改会话时区

```sql
ALTER SESSION SET TIME_ZONE='+08:00';
```





-------------

#### 重启数据库

1. 登陆服务器
2. 切换 oracle 用户 

```
su - root
```

3. 进入 sqlplus 控制台

```
sqlplus /nolog
```

4. 以管理员身份登录

```
conn / as sysdba
```

5. 关闭数据库

```
shutdown immediate
```

6. 重启数据库

```
startup
```

7. 退出控制台

```
exit
```

**如果执行完上面操作后还是连接不上数据库**(ORA-01109: database not open)

+ 进入sqlplus控制台

```
SQL> select con_id,name,open_mode from V$pdbs;

    CON_ID NAME                           OPEN_MODE
---------- ------------------------------ ----------
         3 ORCLPDB1                       MOUNTED

```

+ 将这个 mounted 的容器open

```
SQL> alter pluggable database ORCLPDB1 open;

Pluggable database altered.


```

这样就可以连接到数据库了

-------------------





### 参考

[Oracle 定时作业Job详解](https://blog.csdn.net/xyj0808xyj/article/details/88385570)

[Oracle中的定时任务](https://blog.csdn.net/m0_71406734/article/details/130726256?spm=1001.2014.3001.5506)--very good

[Oracle12报错：ERROR at line 1: ORA-01109: database not open](https://blog.csdn.net/CN_TangZheng/article/details/111479652)