---
title: SQL Learning Note_1
published: 2025-06-26T22:37:57+08:00
summary: SQL basic syntax
cover:
  image: https://i1.hdslb.com/bfs/archive/88f8d10085625622d4c4a774dc4d773bd1205966.jpg
tags:
  - Notes
categories: Learning Journeys
draft: false
lang: zh
---


{{< netease_music "1374617387" >}}

# SQL基础语法总结（基于sqlzoo练习）

https://sqlzoo.net/wiki/SELECT_from_WORLD_Tutorial

## 基础查询结构
```sql

SELECT 列名1, 列名2 AS 别名
FROM 表名
WHERE 条件;

```

## 1. SELECT 和 FROM

- **定义**：`SELECT`用于指定要查询的列，`FROM`用于指定要查询的表。

- **使用条件**：每个查询都必须包含`SELECT`和`FROM`。

- **示例**：

```sql

SELECT name, population  -- 选择特定列
SELECT *                 -- 选择所有列
SELECT area*2 AS doubled_area -- 计算并命名

```

## 2. WHERE

- **定义**：`WHERE`用于过滤记录，只有满足条件的记录才会被返回。

- **使用条件**：放在`FROM`子句之后，可以配合比较运算符（=, <>, >, <等）和逻辑运算符（AND, OR, NOT）使用。

- **示例**：

```sql

SELECT name FROM world WHERE population > 100000000;

```

## 3. LIKE 和 通配符 %

- **定义**：

- `LIKE`用于模式匹配，常与通配符结合使用。

- `%`代表任意长度的任意字符（包括0个字符）。

- **使用条件**：当需要模糊匹配字符串时使用。注意：`LIKE`是大小写敏感的，某些数据库可能需要使用函数（如`LOWER`）来忽略大小写。

- **示例**：

```sql

-- 匹配以'Al'开头的国家

SELECT name FROM world WHERE name LIKE 'Al%';

-- 匹配包含'united'的国家（不区分大小写）

SELECT name FROM world WHERE LOWER(name) LIKE '%united%';

```

## 4. LENGTH

- **定义**：返回字符串的字符长度（字符数）。

- **使用条件**：需要获取字符串长度时使用。注意：在MySQL中，`LENGTH`返回字节数，而`CHAR_LENGTH`返回字符数。在其他数据库中（如PostgreSQL），`LENGTH`返回字符数。

- **示例**：

```sql

SELECT name, LENGTH(name) AS name_length FROM world;
WHERE LENGTH(capital) > 10

```

## 5. ROUND

- **定义**：用于对数值进行四舍五入。

- **使用条件**：需要控制数值的小数位数时使用。可以指定保留的小数位数（正数表示小数位，负数表示整数位）。

- **示例**：

```sql

-- 将人均GDP四舍五入到整数

SELECT name, ROUND(gdp/population) AS per_capita_gdp FROM world;

-- 将面积四舍五入到千位（精确到千）

SELECT name, ROUND(area, -3) AS area_rounded FROM world;

```

## 6. LEFT

- **定义**：从字符串左侧开始提取指定数量的字符。

- **使用条件**：需要提取字符串的前缀时使用。

- **示例**：

```sql

-- 提取国家名称的前3个字符

SELECT name, LEFT(name, 3) AS prefix FROM world;

```


## 7. 比较运算符：`<>` 和 `!=`

- **定义**：两者都表示“不等于”。`<>`是标准SQL运算符，`!=`是非标准但广泛支持的运算符。

- **使用条件**：比较两个值是否不相等。可以用于数值、字符串、日期等。

- **注意**：比较时注意大小写敏感问题，以及NULL值（NULL与任何值比较都返回NULL，需要使用`IS NULL`或`IS NOT NULL`）。

- **示例**：

```sql

-- 排除欧洲国家

SELECT name FROM world WHERE continent <> 'Europe';

-- 排除人口为0的国家

SELECT name FROM world WHERE population != 0;

```

## 8. 逻辑运算符：AND 和 OR

例题：早期医学奖与后期文学奖

**题目**：

展示1910年前（不含1910）的医学奖得主和2004年后（含2004）的文学奖得主的年份、奖项和姓名。

**SQL语句**：

```sql

SELECT yr, subject, winner

FROM nobel

WHERE

(subject = 'Medicine' AND yr < 1910) -- 条件组A：早期医学奖

OR -- 逻辑或

(subject = 'Literature' AND yr >= 2004) -- 条件组B：后期文学奖

```

**解释**：

- 条件组A：`subject = 'Medicine' AND yr < 1910` 筛选出1910年前的医学奖。

- 条件组B：`subject = 'Literature' AND yr >= 2004` 筛选出2004年及以后的文学奖。

- 用 **OR** 连接这两组条件，表示满足任意一组条件的记录都会被选中。

## 9. AS（别名）

- **定义**：为列或表指定临时名称（别名），使结果更易读。

- **使用条件**：在`SELECT`子句中为列重命名，或在`FROM`子句中为表指定别名（简化引用）。

- **示例**：

```sql

-- 为列指定别名

SELECT name AS country, population AS pop FROM world;

-- 为表指定别名（用于自连接或多表查询）

SELECT w.name, w.continent FROM world AS w;

```

## 10. IN

- **核心功能：** 筛选匹配多个值的数据

- **IN 的语法：**

```sql
SELECT 列名1, 列名2, ...
FROM 表名
WHERE 列名 IN (值1, 值2, 值3, ...);
```

- `WHERE 列名 IN (...)`： 这是关键部分。它告诉数据库：“请找出`列名`的值**等于**括号 `(...)` 中列出的**任何一个值**的那些行”
- `(值1, 值2, 值3, ...)`： 这是一个你指定的**值列表**。这些值可以是：
    
    - **数字**： `WHERE StudentID IN (1001, 1005, 1010)`
        
    - **文本（字符串）**： `WHERE City IN ('北京', '上海', '广州')` _(注意：字符串必须用单引号 `''` 括起来！)_
        
    - **日期**： `WHERE OrderDate IN ('2023-10-01', '2023-11-11')` _(日期格式取决于数据库)_
        
    - 列表中的值用**逗号 `,`** 分隔。


