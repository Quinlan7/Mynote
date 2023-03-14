+ 在Oracle里，对表使用别名后，凡是用到表名的地方都只能使用别名。 否则报错，

+ oracle的别名如果不用双引号括起来的话，会自动把别名都变为大写，用双引号括起来才会区分大小写
+ oracle 对查询结果列起别名时，可以直接用中文名，用不用双引号都可以，但是不能用单引号
+ oracle 对分类查询内容命名时要用单引号
  + 下例中的 已完成 必须用单引号包裹，否则报错，无论是双引号还是直接写都会报错，可能是因为是每一行的内容不是列名，所以要求不一样

```sql
SELECT
	S.XM,
	S.XH,
	S.BJ,
	A.PRODUCT_NAME,
	( CASE WHEN Q.STATUS = 1 THEN '已完成' END ) 状态
FROM
	"QUESTIONNAIRE_TEST" Q
	INNER JOIN STU_UNDERGRADUATE_INFO S ON S.XH = Q.STU_ID
	INNER JOIN QUESTIONNAIRE_PRODUCT A ON A.PRODUCT_CODE = Q.PRODUCT_CODE 
WHERE
	Q.STATUS = 1 
ORDER BY
	XH
	
```

