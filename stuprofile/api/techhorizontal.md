[toc]
# 教师分析页面

### 获取老师对应画报的四个维度列表（api：/horizontalContrast/dimensionList）

##### 传入参数

![image-20221116191515886](techhorizontal.assets/image-20221116191515886.png)

##### 返回参数实例

```json
{
  "code": 200,
  "msg": "成功",
  "data": [
    [
      {
        "majorId": "1401",
        "majorName": "电气工程及其自动化"
      },
      {
        "majorId": "2823",
        "majorName": "新能源科学与工程"
      }
    ],
    [
      {
        "classId": "837",
        "className": "新能201"
      },
      {
        "classId": "845",
        "className": "电气2008"
      },
      {
        "classId": "846",
        "className": "电气2009"
      },
      {
        "classId": "848",
        "className": "电气2010"
      },
      {
        "classId": "860",
        "className": "电气2007"
      },
      {
        "classId": "974",
        "className": "电气2004"
      },
      {
        "classId": "985",
        "className": "新能202"
      },
      {
        "classId": "986",
        "className": "电气2001"
      },
      {
        "classId": "998",
        "className": "电气2005"
      },
      {
        "classId": "999",
        "className": "电气2006"
      },
      {
        "classId": "1008",
        "className": "电气2003"
      },
      {
        "classId": "872",
        "className": "电气US202"
      },
      {
        "classId": "954",
        "className": "电气US204"
      },
      {
        "classId": "955",
        "className": "电气US201"
      },
      {
        "classId": "956",
        "className": "电气US203"
      },
      {
        "classId": "962",
        "className": "电气2002"
      }
    ],
    [
      {
        "dormitoryName": "红桥校区红桥东院东十二1008",
        "dormitoryId": "红桥校区红桥东院东十二1008"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二302",
        "dormitoryId": "红桥校区红桥东院东十二302"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五114",
        "dormitoryId": "红桥校区红桥东院东五114"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二1006",
        "dormitoryId": "红桥校区红桥东院东十二1006"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十一319",
        "dormitoryId": "红桥校区红桥东院东十一319"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十906",
        "dormitoryId": "红桥校区红桥东院东十906"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四410",
        "dormitoryId": "红桥校区红桥东院东四410"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十607",
        "dormitoryId": "红桥校区红桥东院东十607"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四401",
        "dormitoryId": "红桥校区红桥东院东四401"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十608",
        "dormitoryId": "红桥校区红桥东院东十608"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十712",
        "dormitoryId": "红桥校区红桥东院东十712"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十715",
        "dormitoryId": "红桥校区红桥东院东十715"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四408",
        "dormitoryId": "红桥校区红桥东院东四408"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二202",
        "dormitoryId": "红桥校区红桥东院东十二202"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四403",
        "dormitoryId": "红桥校区红桥东院东四403"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十708",
        "dormitoryId": "红桥校区红桥东院东十708"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二203",
        "dormitoryId": "红桥校区红桥东院东十二203"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十312",
        "dormitoryId": "红桥校区红桥东院东十312"
      },
      {
        "dormitoryName": "红桥校区红桥东院南四910",
        "dormitoryId": "红桥校区红桥东院南四910"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十309",
        "dormitoryId": "红桥校区红桥东院东十309"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十904",
        "dormitoryId": "红桥校区红桥东院东十904"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十601",
        "dormitoryId": "红桥校区红桥东院东十601"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十916",
        "dormitoryId": "红桥校区红桥东院东十916"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十610",
        "dormitoryId": "红桥校区红桥东院东十610"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四411",
        "dormitoryId": "红桥校区红桥东院东四411"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十612",
        "dormitoryId": "红桥校区红桥东院东十612"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五423",
        "dormitoryId": "红桥校区红桥东院东五423"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十314",
        "dormitoryId": "红桥校区红桥东院东十314"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十710",
        "dormitoryId": "红桥校区红桥东院东十710"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十602",
        "dormitoryId": "红桥校区红桥东院东十602"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十210",
        "dormitoryId": "红桥校区红桥东院东十210"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十310",
        "dormitoryId": "红桥校区红桥东院东十310"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十一1015",
        "dormitoryId": "红桥校区红桥东院东十一1015"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十319",
        "dormitoryId": "红桥校区红桥东院东十319"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四405",
        "dormitoryId": "红桥校区红桥东院东四405"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十316",
        "dormitoryId": "红桥校区红桥东院东十316"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十609",
        "dormitoryId": "红桥校区红桥东院东十609"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四412",
        "dormitoryId": "红桥校区红桥东院东四412"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五425",
        "dormitoryId": "红桥校区红桥东院东五425"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三227",
        "dormitoryId": "红桥校区红桥东院东三227"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十301",
        "dormitoryId": "红桥校区红桥东院东十301"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三406",
        "dormitoryId": "红桥校区红桥东院东三406"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十613",
        "dormitoryId": "红桥校区红桥东院东十613"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十一1011",
        "dormitoryId": "红桥校区红桥东院东十一1011"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十709",
        "dormitoryId": "红桥校区红桥东院东十709"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十706",
        "dormitoryId": "红桥校区红桥东院东十706"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十306",
        "dormitoryId": "红桥校区红桥东院东十306"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二204",
        "dormitoryId": "红桥校区红桥东院东十二204"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十414",
        "dormitoryId": "红桥校区红桥东院东十414"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十606",
        "dormitoryId": "红桥校区红桥东院东十606"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十719",
        "dormitoryId": "红桥校区红桥东院东十719"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十317",
        "dormitoryId": "红桥校区红桥东院东十317"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十703",
        "dormitoryId": "红桥校区红桥东院东十703"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三407",
        "dormitoryId": "红桥校区红桥东院东三407"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三325",
        "dormitoryId": "红桥校区红桥东院东三325"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四404",
        "dormitoryId": "红桥校区红桥东院东四404"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十214",
        "dormitoryId": "红桥校区红桥东院东十214"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十615",
        "dormitoryId": "红桥校区红桥东院东十615"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十304",
        "dormitoryId": "红桥校区红桥东院东十304"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四407",
        "dormitoryId": "红桥校区红桥东院东四407"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十409",
        "dormitoryId": "红桥校区红桥东院东十409"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二1002",
        "dormitoryId": "红桥校区红桥东院东十二1002"
      },
      {
        "dormitoryName": "红桥校区红桥东院东九405",
        "dormitoryId": "红桥校区红桥东院东九405"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十212",
        "dormitoryId": "红桥校区红桥东院东十212"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五113",
        "dormitoryId": "红桥校区红桥东院东五113"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五115",
        "dormitoryId": "红桥校区红桥东院东五115"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五116",
        "dormitoryId": "红桥校区红桥东院东五116"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三312",
        "dormitoryId": "红桥校区红桥东院东三312"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三225",
        "dormitoryId": "红桥校区红桥东院东三225"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三223",
        "dormitoryId": "红桥校区红桥东院东三223"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十614",
        "dormitoryId": "红桥校区红桥东院东十614"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五504",
        "dormitoryId": "红桥校区红桥东院东五504"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十305",
        "dormitoryId": "红桥校区红桥东院东十305"
      },
      {
        "dormitoryName": "红桥校区红桥南院南四217",
        "dormitoryId": "红桥校区红桥南院南四217"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十313",
        "dormitoryId": "红桥校区红桥东院东十313"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十315",
        "dormitoryId": "红桥校区红桥东院东十315"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四409",
        "dormitoryId": "红桥校区红桥东院东四409"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十716",
        "dormitoryId": "红桥校区红桥东院东十716"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十913",
        "dormitoryId": "红桥校区红桥东院东十913"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二301",
        "dormitoryId": "红桥校区红桥东院东十二301"
      },
      {
        "dormitoryName": "红桥校区红桥南院南四215",
        "dormitoryId": "红桥校区红桥南院南四215"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十303",
        "dormitoryId": "红桥校区红桥东院东十303"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十307",
        "dormitoryId": "红桥校区红桥东院东十307"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十308",
        "dormitoryId": "红桥校区红桥东院东十308"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十311",
        "dormitoryId": "红桥校区红桥东院东十311"
      },
      {
        "dormitoryName": "红桥校区红桥东院东四406",
        "dormitoryId": "红桥校区红桥东院东四406"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十905",
        "dormitoryId": "红桥校区红桥东院东十905"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十604",
        "dormitoryId": "红桥校区红桥东院东十604"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二1001",
        "dormitoryId": "红桥校区红桥东院东十二1001"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十914",
        "dormitoryId": "红桥校区红桥东院东十914"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十211",
        "dormitoryId": "红桥校区红桥东院东十211"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十213",
        "dormitoryId": "红桥校区红桥东院东十213"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十711",
        "dormitoryId": "红桥校区红桥东院东十711"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五506",
        "dormitoryId": "红桥校区红桥东院东五506"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十611",
        "dormitoryId": "红桥校区红桥东院东十611"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十二1009",
        "dormitoryId": "红桥校区红桥东院东十二1009"
      },
      {
        "dormitoryName": "红桥校区红桥南院南四219",
        "dormitoryId": "红桥校区红桥南院南四219"
      },
      {
        "dormitoryName": "红桥校区红桥东院东五119",
        "dormitoryId": "红桥校区红桥东院东五119"
      },
      {
        "dormitoryName": "红桥校区红桥东院东三405",
        "dormitoryId": "红桥校区红桥东院东三405"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十603",
        "dormitoryId": "红桥校区红桥东院东十603"
      },
      {
        "dormitoryName": "红桥校区红桥东院东十605",
        "dormitoryId": "红桥校区红桥东院东十605"
      }
    ]
  ]
}
```



### 横向对比饼状图（api：/zhfTemp/horiztontalpiechart）

##### 传入参数的说明

<img src="techhorizontal.assets/image-20221116151418930.png" alt="image-20221116151418930" style="zoom:80%;" />

##### 传入参数的实例

```json
{
  "accountId": "2021042",
  "dimensionId": "0",
  "intentionId": "1",
  "roleId": "1397533427003101186"
}
```

##### 返回参数实例

```json
{
  "code": 200,
  "msg": "成功",
  "data": [
    {
      "VALUE": 11,
      "TYPE": "新能源科学与工程"
    },
    {
      "VALUE": 56,
      "TYPE": "电气工程及其自动化"
    }
  ]
}
```

### 横向对比叠装图（api：/zhfTemp/horiztontalhistogram）

##### 传入参数说明及实例（GET方法）

<img src="techhorizontal.assets/image-20221116141739965.png" alt="image-20221116141739965" style="zoom:33%;" />

##### 返回参数实例

```json
{
  "code": 200,
  "msg": "成功",
  "data": [
    [
      {
        "INTENTION": "灵活就业",
        "VALUE": 5,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 7,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 7,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "考研",
        "VALUE": 7,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "考公",
        "VALUE": 8,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 11,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 11,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 12,
        "TYPE": "新能源科学与工程"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 46,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "考研",
        "VALUE": 49,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 55,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "考公",
        "VALUE": 55,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 56,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 56,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 56,
        "TYPE": "电气工程及其自动化"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 66,
        "TYPE": "电气工程及其自动化"
      }
    ],
    [
      {
        "INTENTION": "迷茫",
        "VALUE": 9,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 7,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 4,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 4,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "考公",
        "VALUE": 5,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "考研",
        "VALUE": 3,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 4,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 6,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 6,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "考研",
        "VALUE": 6,
        "TYPE": "电气US203"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 5,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 4,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "考公",
        "VALUE": 6,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "电气US203"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "考研",
        "VALUE": 6,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 5,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 5,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 3,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 4,
        "TYPE": "电气US203"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "电气US203"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 11,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 5,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "考研",
        "VALUE": 6,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 5,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 5,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 5,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 4,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 3,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 3,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 5,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 6,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 5,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 5,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 6,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 7,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 4,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 6,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 4,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 4,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "电气US203"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 5,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 6,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 5,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 4,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 5,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 3,
        "TYPE": "电气2009"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 3,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 3,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 7,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 5,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 3,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 5,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 3,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "电气2002"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 6,
        "TYPE": "新能201"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 5,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 7,
        "TYPE": "电气2010"
      },
      {
        "INTENTION": "考研",
        "VALUE": 3,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 3,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "考公",
        "VALUE": 5,
        "TYPE": "新能202"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 6,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 7,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "考公",
        "VALUE": 8,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 3,
        "TYPE": "电气2001"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 5,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "考研",
        "VALUE": 4,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 5,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 4,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 7,
        "TYPE": "电气US203"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "电气2008"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 5,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "考公",
        "VALUE": 6,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 7,
        "TYPE": "电气2007"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 5,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 3,
        "TYPE": "电气2004"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 9,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "考公",
        "VALUE": 7,
        "TYPE": "电气2005"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 7,
        "TYPE": "电气2006"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 6,
        "TYPE": "电气2003"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 5,
        "TYPE": "电气US202"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "电气US204"
      },
      {
        "INTENTION": "考公",
        "VALUE": 5,
        "TYPE": "电气US201"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "电气US203"
      }
    ],
    [
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十712"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五115"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五116"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十715"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十716"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十719"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十719"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十319"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四408"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十317"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十317"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十610"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十611"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十611"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二1008"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四219"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三312"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东三312"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东三225"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三223"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四404"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四404"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十301"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十214"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十214"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三406"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五423"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五423"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十614"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十614"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十615"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十709"
      },
      {
        "INTENTION": "考研",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东五114"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十307"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二203"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十312"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十一319"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四407"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四405"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十304"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五115"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十710"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十711"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十602"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十603"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十603"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十604"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十605"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十210"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十211"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五113"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十706"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二1006"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十608"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二204"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十316"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十一1015"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十601"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五115"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五116"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十609"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十611"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三407"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五425"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五425"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五425"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四404"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十301"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三405"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十615"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十615"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 4,
        "TYPE": "红桥校区红桥东院东十二301"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十708"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十307"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十308"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四407"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十312"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十314"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四405"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十710"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四406"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东四409"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四410"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十604"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五116"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十914"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十二204"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十601"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十601"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十210"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十210"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十211"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十706"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四217"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十609"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十609"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十310"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五425"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四406"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十712"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五115"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五115"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十715"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十716"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十316"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十317"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十610"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十610"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十611"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十703"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十612"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东三227"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东三227"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三225"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十214"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十301"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三407"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东三406"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三406"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三405"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三325"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十706"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十304"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十305"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十308"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四407"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十313"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十313"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十313"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十315"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十710"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十710"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十711"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四215"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四406"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十308"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十904"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十904"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十603"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十604"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十605"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十309"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四409"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十414"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十211"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十211"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十212"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四410"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十608"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十310"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十315"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十706"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十712"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十716"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十716"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五506"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十611"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十913"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四411"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四403"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三227"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十301"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十303"
      },
      {
        "INTENTION": "考研",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东三407"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三405"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二302"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四403"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四403"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十613"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十614"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十一1011"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五504"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五114"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十305"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十306"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十307"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥南院南四217"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十313"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十314"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十315"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十710"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院南四910"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二203"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十904"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四410"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十601"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二202"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十606"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十607"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十607"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十309"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十310"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四217"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十319"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十319"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四408"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十316"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十316"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十二301"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十612"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三225"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四404"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五119"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十214"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三406"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十613"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十613"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十614"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五506"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十708"
      },
      {
        "INTENTION": "考公",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十709"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十305"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十306"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十307"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十905"
      },
      {
        "INTENTION": "考研",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十906"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十603"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四217"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十409"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十409"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二204"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四409"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十916"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十213"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十213"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二202"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二203"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五113"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五113"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五114"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十606"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四401"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十608"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十609"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十311"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十913"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五504"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十306"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十313"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十711"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十712"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五116"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十319"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四408"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五506"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十610"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十610"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东四411"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四412"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四215"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五425"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东三312"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三227"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四404"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十301"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三405"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三325"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五423"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十612"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十613"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五504"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五504"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十708"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十709"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十305"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十305"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十306"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十307"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十311"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十312"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十314"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四405"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十710"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二203"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二203"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四406"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五119"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十904"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十905"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十906"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十603"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十603"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十605"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十309"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十309"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十409"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二1001"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十414"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十601"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十210"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十212"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十213"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二202"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十311"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十605"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十607"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十310"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十316"
      },
      {
        "INTENTION": "考研",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十715"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十719"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十319"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四405"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四408"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十317"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十610"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十612"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四403"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五423"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东三325"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十214"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十303"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三405"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三405"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东五423"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十306"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四219"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四407"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十313"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十314"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五114"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四406"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十309"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十602"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十904"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十906"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十602"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十604"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十409"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十212"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二203"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五113"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五113"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 3,
        "TYPE": "红桥校区红桥东院东十606"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四410"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四411"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十608"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥南院南四215"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二1009"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 2,
        "TYPE": "红桥校区红桥南院南四219"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四412"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十612"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三312"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三225"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三223"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三406"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十615"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五504"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五506"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五506"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东三407"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十708"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东五119"
      },
      {
        "INTENTION": "迷茫",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十304"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十308"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二1006"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十312"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥南院南四215"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四407"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十315"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四405"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十303"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十304"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十905"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十703"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十二204"
      },
      {
        "INTENTION": "灵活就业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四410"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十二1002"
      },
      {
        "INTENTION": "应征入伍",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东九405"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十414"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十414"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十414"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十210"
      },
      {
        "INTENTION": "考公",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十211"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 2,
        "TYPE": "红桥校区红桥东院东十212"
      },
      {
        "INTENTION": "考公",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十212"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十213"
      },
      {
        "INTENTION": "自主创业",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东四411"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十310"
      },
      {
        "INTENTION": "找工作",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十311"
      },
      {
        "INTENTION": "考研",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十311"
      },
      {
        "INTENTION": "出国留学",
        "VALUE": 1,
        "TYPE": "红桥校区红桥东院东十317"
      }
    ]
  ]
}
```



### 横向对比折线图 （api：/zhfTemp/horiztontallinechart）

##### 传入参数说明（POST方法）

![image-20221116134659064](techhorizontal.assets/image-20221116134659064.png)

##### 专业维度传入参数实例

![image-20221116134823847](techhorizontal.assets/image-20221116134823847.png)

```json
{
  "accountId": "2021042",
  "dimensionId": "0",
  "id": "1401",
  "roleId": "1397533427003101186"
}
```

##### 班级维度传入参数实例

![image-20221116134302110](techhorizontal.assets/image-20221116134302110.png)

```json
{
  "accountId": "2021042",
  "dimensionId": "1",
  "id": "837",
  "roleId": "1397533427003101186"
}
```



##### 宿舍维度传入参数实例

![image-20221116135034367](techhorizontal.assets/image-20221116135034367.png)

```json
{
  "accountId": "2021042",
  "dimensionId": "2",
  "id": "红桥校区红桥东院东五504",
  "roleId": "1397533427003101186"
}
```



##### 返回结果实例(班级维度，其他维度基本一致)

```json
{
  "code": 200,
  "msg": "成功",
  "data": [
    {
      "total": 6,
      "time": "2022-11-16",
      "intentionState": "找工作-已签三方"
    },
    {
      "total": 1,
      "time": "2022-11-16",
      "intentionState": "考研-已录取"
    },
    {
      "total": 2,
      "time": "2022-11-16",
      "intentionState": "入伍-已公示"
    },
    {
      "total": 5,
      "time": "2022-11-16",
      "intentionState": "迷茫-已被老师指导"
    },
    {
      "total": 3,
      "time": "2022-11-16",
      "intentionState": "创业-创业成功"
    },
    {
      "total": 2,
      "time": "2022-11-16",
      "intentionState": "灵活就业-已就业"
    },
    {
      "total": 6,
      "time": "2022-11-09",
      "intentionState": "找工作-已签三方"
    },
    {
      "total": 1,
      "time": "2022-11-09",
      "intentionState": "考研-已录取"
    },
    {
      "total": 2,
      "time": "2022-11-09",
      "intentionState": "入伍-已公示"
    },
    {
      "total": 5,
      "time": "2022-11-09",
      "intentionState": "迷茫-已被老师指导"
    },
    {
      "total": 3,
      "time": "2022-11-09",
      "intentionState": "创业-创业成功"
    },
    {
      "total": 2,
      "time": "2022-11-09",
      "intentionState": "灵活就业-已就业"
    },
    {
      "total": 6,
      "time": "2022-11-02",
      "intentionState": "找工作-已签三方"
    },
    {
      "total": 1,
      "time": "2022-11-02",
      "intentionState": "考研-已录取"
    },
    {
      "total": 2,
      "time": "2022-11-02",
      "intentionState": "入伍-已公示"
    },
    {
      "total": 5,
      "time": "2022-11-02",
      "intentionState": "迷茫-已被老师指导"
    },
    {
      "total": 3,
      "time": "2022-11-02",
      "intentionState": "创业-创业成功"
    },
    {
      "total": 2,
      "time": "2022-11-02",
      "intentionState": "灵活就业-已就业"
    },
    {
      "total": 6,
      "time": "2022-10-26",
      "intentionState": "找工作-已签三方"
    },
    {
      "total": 1,
      "time": "2022-10-26",
      "intentionState": "考研-已录取"
    },
    {
      "total": 2,
      "time": "2022-10-26",
      "intentionState": "入伍-已公示"
    },
    {
      "total": 5,
      "time": "2022-10-26",
      "intentionState": "迷茫-已被老师指导"
    },
    {
      "total": 3,
      "time": "2022-10-26",
      "intentionState": "创业-创业成功"
    },
    {
      "total": 2,
      "time": "2022-10-26",
      "intentionState": "灵活就业-已就业"
    },
    {
      "total": 6,
      "time": "2022-10-19",
      "intentionState": "找工作-已签三方"
    },
    {
      "total": 1,
      "time": "2022-10-19",
      "intentionState": "考研-已录取"
    },
    {
      "total": 2,
      "time": "2022-10-19",
      "intentionState": "入伍-已公示"
    },
    {
      "total": 5,
      "time": "2022-10-19",
      "intentionState": "迷茫-已被老师指导"
    },
    {
      "total": 3,
      "time": "2022-10-19",
      "intentionState": "创业-创业成功"
    },
    {
      "total": 2,
      "time": "2022-10-19",
      "intentionState": "灵活就业-已就业"
    },
    {
      "total": 6,
      "time": "2022-10-12",
      "intentionState": "找工作-已签三方"
    },
    {
      "total": 1,
      "time": "2022-10-12",
      "intentionState": "考研-已录取"
    },
    {
      "total": 2,
      "time": "2022-10-12",
      "intentionState": "入伍-已公示"
    },
    {
      "total": 5,
      "time": "2022-10-12",
      "intentionState": "迷茫-已被老师指导"
    },
    {
      "total": 3,
      "time": "2022-10-12",
      "intentionState": "创业-创业成功"
    },
    {
      "total": 2,
      "time": "2022-10-12",
      "intentionState": "灵活就业-已就业"
    }
  ]
}
```