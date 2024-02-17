## 字符函数

SUBSTRING 允许您提取字符串的一部分
`SUBSTRING('Hello world', 2, 3) -> 'ell' `

LEFT（s，n） 允许您从字符串 s 的开头提取 n 个字符
`LEFT('Hello world', 4) -> 'Hell'`

RIGHT（s，n） 允许您从字符串 s 的末尾提取 n 个字符。
`RIGHT('Hello world', 4) -> 'orld'`


CONCAT(string1, string2, ...)
string1、string2等是要连接的字符串
`例如，将"Hello"和"World"连接在一起：`
`SELECT CONCAT('Hello', ' ', 'World')`
返回一个结果为"Hello World"的字符串

```sql
显示欧洲每个国家的名称和人口。显示人口占德国人口的百分比
SELECT
  name,
  CONCAT(
    ROUND(
      population /(
        SELECT
          population
        FROM
          world
        WHERE
          name = 'Germany'
      ) * 100,
      0
    ),
    '%'
  ) AS population
FROM
  world
WHERE
  continent = 'Europe'
```
## 数值函数

ROUND函数用于将数值四舍五入到指定的小数位数：
ROUND(number, num_digits)
number是要四舍五入的数值
num_digits是要保留的小数位数
`例如，将数字3.1415四舍五入到小数点后第二位：`
`SELECT ROUND(3.1415, 2);`
返回结果为3.14

MAX函数用于返回指定列中的最大值：
```sql
SELECT MAX(column_name) FROM table_name;
column_name是要查找最大值的列名
table_name是包含该列的表名
```
可以用这个词 ALL 来允许 >= 或 > 或 < 或 <= 对列表采取行动
```sql
查找每个大洲中最大的国家（按面积），显示大洲、名称和区域
SELECT continent, name, area FROM world x
  WHERE area>= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)

```

## 条件函数
```sql
SELECT
device_id,gender,
        CASE
            WHEN age<20 THEN '20岁以下'
            WHEN age BETWEEN 20 AND 24 THEN '20-24岁'
            WHEN age>=25 THEN '25岁及以上'
            WHEN age IS NULL THEN '其他'
            END AS age_cut
FROM user_profile
-- 内嵌case语句用法
/*多重数值条件判断使用,也可以用if嵌套*/
SELECT device_id,gender,
  IF(age is null,'其他',
     IF(age<20,'20岁以下',
        IF(age<=24,'20-24岁','25岁及以上'))) age_cut
FROM user_profile
```

## 日期函数

```sql
SELECT
    day(date) as day,
    count(question_id) as question_cnt
FROM question_practice_detail
WHERE month(date)=8 and year(date)=2021
GROUP BY date;
-- 对于日期函数的基本使用
```

```sql
SELECT
  name,
  CONCAT(
    ROUND(
      population /(
        SELECT
          population
        FROM
          world
        WHERE
          name = 'Germany'
      ) * 100,
      0
    ),
    '%'
  ) AS population
FROM
  world
WHERE
  continent = 'Europe'
```