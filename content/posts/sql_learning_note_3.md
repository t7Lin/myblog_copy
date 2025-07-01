---
title: SQL Learning Note_3
published: 2025-06-28T23:51:57+08:00
summary: SQL syntax learning based on sqlzoo
cover:
  image: https://i1.hdslb.com/bfs/archive/88f8d10085625622d4c4a774dc4d773bd1205966.jpg
tags:
  - Notes
categories: Learning Journeys
draft: false
lang:
---

https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial

### 📌 **核心函数与运算符**

| 关键词        | 定义                    | 使用场景                  | 示例                                                 |
| ---------- | --------------------- | --------------------- | -------------------------------------------------- |
| `CONCAT()` | 连接字符串                 | 组合文本和计算结果             | `CONCAT(ROUND(ratio*100), '%') → '25%'`            |
| `%`        | 1. 数学取模  <br>2. 百分比符号 | 1. 奇偶判断  <br>2. 显示百分比 | 1. `population % 2 = 0`（偶人口）  <br>2. `11%`（最终显示格式） |
### 🔍 **分组与筛选**

|子句|定义|执行顺序|使用场景|示例|
|---|---|---|---|---|
|`GROUP BY`|按列分组|2|分组统计|`SELECT continent, COUNT(*) FROM world GROUP BY continent`|
|`HAVING`|筛选分组后的结果|3|对聚合结果过滤|`HAVING AVG(population) > 10000000`（筛选平均人口>1000万的洲）|
|`WHERE`|筛选原始数据行|1|聚合前的数据过滤|`WHERE continent = 'Europe'`（仅处理欧洲国家）|

> **执行顺序**：`WHERE` → `GROUP BY` → `HAVING`

### 📊 **聚合函数**

| 函数        | 定义   | 使用场景   | 示例                           |
| --------- | ---- | ------ | ---------------------------- |
| `COUNT()` | 统计行数 | 计算数量   | `COUNT(*)`（总行数）              |
| `SUM()`   | 求和   | 计算总和   | `SUM(gdp)`（GDP总和）            |
| `AVG()`   | 求平均值 | 计算平均水平 | `AVG(population)`（平均人口）      |
| `MAX()`   | 求最大值 | 找最高值   | `MAX(life_expectancy)`（最长寿命） |
| `MIN()`   | 求最小值 | 找最低值   | `MIN(area)`（最小面积）            |
### 🔄 **子查询 (Subqueries)**

| 类型    | 定义         | 使用场景        | 示例                                                                                                                      |
| ----- | ---------- | ----------- | ----------------------------------------------------------------------------------------------------------------------- |
| 标量子查询 | 返回单个值      | 作为比较对象或计算参数 | `SELECT name FROM world WHERE population > (SELECT population FROM ...)`                                                |
| 列子查询  | 返回一列值      | 与`IN`搭配使用   | `SELECT name FROM world WHERE continent IN (SELECT continent FROM ...)`                                                 |
| 关联子查询 | 引用外部查询的子查询 | 复杂的分组比较     | `SELECT name FROM world w1 WHERE population > (SELECT AVG(population) FROM world w2 WHERE w2.continent = w1.continent)` |
### 🧩 **关键技巧总结**

1. **去重显示** → 用 `DISTINCT`
    ```sql
    SELECT DISTINCT continent FROM world
```
 
2. **百分比计算** → 子查询 + 数学运算
    ```sql
    100 * population / (SELECT population FROM world WHERE name='Germany')
```

3. **条件排序** → `CASE` + `ORDER BY`
```sql
ORDER BY CASE WHEN name IN ('A','B') THEN 0 ELSE 1 END
```
    
4. **范围筛选** → 双子查询比较
 ```sql
 WHERE population BETWEEN (SELECT ...) AND (SELECT ...)
```

5. **分组后筛选** → `GROUP BY` + `HAVING`
    ```sql
    SELECT continent 
    FROM world 
    GROUP BY continent 
    HAVING COUNT(*) > 10
```
