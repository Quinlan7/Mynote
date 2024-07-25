第一个sql 140s，第二个0.6s
原因是这个表会每分钟插入200条数据 sql执行时间不稳定


```sql
SELECT
  ROUND( AVG( S.CUR_WATER_HEIGHT ), 2 ) AS avg_water_height 
FROM
  WATERCULVERT.M_SCINFO S 
WHERE
  S.CULVERTS_UUID = 'DD002' 
  AND TO_DATE( S.LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS' ) >= SYSDATE - INTERVAL '2' HOUR 
  AND TO_DATE( S.LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS' ) <= SYSDATE 
  




SELECT
  ROUND( AVG( CUR_WATER_HEIGHT ), 2 ) AS avg_water_height 
FROM
  (
    SELECT 
      S.CUR_WATER_HEIGHT 
    FROM
      WATERCULVERT.M_SCINFO S 
    WHERE
      S.CULVERTS_UUID = 'DD002' 
      AND TO_DATE( S.LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS' ) >= SYSDATE - INTERVAL '2' HOUR 
      AND TO_DATE( S.LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS' ) <= SYSDATE
  );

```

使用物化视图使得，检索和插入分离

```sql
--========================= 物化视图创建 ============================
-- 物化视图只能在对应模式的用户下创建，即使是DBA页无权创建非对应模式的视图
-- 使用物化视图 直接把重复数据去除
CREATE MATERIALIZED VIEW mv_avg_water_height
BUILD IMMEDIATE
REFRESH COMPLETE ON DEMAND
AS
SELECT
  CULVERTS_UUID,
  ROUND(AVG(CUR_WATER_HEIGHT), 2) AS avg_water_height,
  TO_DATE(LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS') AS LAST_REPORT_DT_DATE
FROM
  WATERCULVERT.M_SCINFO
GROUP BY
  CULVERTS_UUID,
  TO_DATE(LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS');


--========================= 定时任务 ============================
-- 查看当前用户
select * from user_users;
-- 查看当前用户权限
SELECT * FROM session_roles;
-- 给对应用户定时任务的创建与管理权限
GRANT CREATE JOB TO WATERCULVERT;
GRANT MANAGE SCHEDULER TO WATERCULVERT;

-- 先测试对应的定时任务，是否可以正常执行，因为即使语句有问题，只要你的创建定时任务的语句没问题，oracle也不会报错
-- 所以一定先确定你的任务是正确的
BEGIN DBMS_MVIEW.REFRESH('mv_avg_water_height', 'C'); END;
-- 创建定时任务，每小时刷新物化视图
BEGIN
  DBMS_SCHEDULER.create_job (
    job_name        => 'refresh_mv_avg_water_height',
    job_type        => 'PLSQL_BLOCK',
    job_action      => 'BEGIN DBMS_MVIEW.REFRESH(''mv_avg_water_height'', ''C''); END;',
    start_date      => SYSTIMESTAMP,
    repeat_interval => 'FREQ=HOURLY; INTERVAL=1', -- 每小时执行一次
    enabled         => TRUE
  );
END;

-- 删除定时任务
BEGIN
  DBMS_SCHEDULER.DROP_JOB('REFRESH_MV_AVG_WATER_HEIGHT');
END;

--========================= 效果测试 ============================
-- 原sql   140s
SELECT
  ROUND( AVG( CUR_WATER_HEIGHT ), 2 ) AS avg_water_height
FROM
  (
    SELECT
      S.CUR_WATER_HEIGHT
    FROM
      WATERCULVERT.M_SCINFO S
    WHERE
      S.CULVERTS_UUID = 'DD005'
      AND TO_DATE( S.LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS' ) >= SYSDATE - INTERVAL '2' HOUR
      AND TO_DATE( S.LAST_REPORT_DT, 'YYYY-MM-DD HH24:MI:SS' ) <= SYSDATE
  );

-- 使用物化视图的sql     0.6s
SELECT
  ROUND( AVG( avg_water_height ), 2 ) AS avg_water_height
FROM
  (
    SELECT
  avg_water_height
FROM
  mv_avg_water_height
WHERE
  CULVERTS_UUID = 'DD032'
  AND LAST_REPORT_DT_DATE >= SYSDATE - INTERVAL '2' HOUR
  AND LAST_REPORT_DT_DATE <= SYSDATE
  );

```

