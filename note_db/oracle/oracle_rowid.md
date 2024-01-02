# oracle 数据库去重

*oracle 数据库可以不指定主键，那么完全相同的元素我们要如何去重呢？oracle有自带的行唯一标识 ROWID*



## 简介

rowid是一个用来唯一标记表中行的伪列。它是物理表中行数据的内部地址，包含两个地址，其一为指向数据表中包含该行的块所存放数据文件的地址，另一个是可以直接定位到数据行自身的这一行在数据块中的地址。

## 利用ROWID进行数据库的去重操作

通过定时任务，每天凌晨两点对前一天的数据进行去重

```sql
--删除前一天重复数据的存储过程
CREATE OR REPLACE PROCEDURE DEDUPLICATION_ETC_EXIT AS
BEGIN
  DELETE FROM "ETCTS_EXIT"
  WHERE (EXTIME >= TRUNC(SYSDATE) - 1 AND EXTIME < TRUNC(SYSDATE))
  AND ROWID NOT IN (
    SELECT MAX(ROWID)
    FROM "ETCTS_EXIT" e
    WHERE EXTIME >= TRUNC(SYSDATE) - 1
    AND EXTIME < TRUNC(SYSDATE)
    GROUP BY PASSID, ID, VEHICLEID
  );

  COMMIT;
END;

--然后创建 scheduler 定时任务
BEGIN
  DBMS_SCHEDULER.CREATE_JOB (
    job_name        => 'DEDUPLICATION_ETCEXIT_SCHEDULER',
    job_type        => 'STORED_PROCEDURE',
    job_action      => 'DEDUPLICATION_ETC_EXIT',
    start_date      => SYSDATE,
    repeat_interval => 'FREQ=DAILY; BYHOUR=2; BYMINUTE=0; BYSECOND=0',
    enabled         => TRUE,
    comments        => '清理过期数据');
END;
```