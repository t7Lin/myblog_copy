---
title: SQL Learning Note_2
published: 2025-06-27T13:02:57+08:00
summary: SQL syntax learning based on sqlzoo
cover:
  image: https://i1.hdslb.com/bfs/archive/88f8d10085625622d4c4a774dc4d773bd1205966.jpg
tags:
  - Notes
categories: Learning Journeys
draft: false
lang:
---
{{< netease_music "1461559218" >}}


### **SQL 排序核心总结 (`ORDER BY` + `ASC`/`DESC`)**

https://sqlzoo.net/wiki/SELECT_from_Nobel_Tutorial

#### 1. 基本定义

| 关键字    | 含义              | 默认  | 排序方向        | 示例                        |
| ------ | --------------- | --- | ----------- | ------------------------- |
| `ASC`  | 升序 (Ascending)  | 是   | 从小到大 / 从早到晚 | `A→Z` `1→100` `2000→2023` |
| `DESC` | 降序 (Descending) | 否   | 从大到小 / 从晚到早 | `Z→A` `100→1` `2023→2000` |
#### 2.核心语法
```sql
SELECT 列1, 列2
FROM 表名
ORDER BY 
  排序列1 [ASC|DESC],  -- 第一优先级
  排序列2 [ASC|DESC]   -- 第二优先级（当列1值相同时生效）
```
#### 3.经典例题

- Knights in order

List the winners, year and subject where the winner starts with **Sir**. Show the the most recent first, then by name order.


```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'         -- 筛选骑士
ORDER BY yr DESC, winner ASC;    -- 先年份降序，再姓名升序
```

- **结果示例**

| winner              | yr   | subject   | 排序逻辑说明      |
| ------------------- | ---- | --------- | ----------- |
| Sir Peter Ratcliffe | 2019 | Medicine  | 年份最新 (2019) |
| Sir Gregory Winter  | 2018 | Chemistry | ↓ 年份降序      |
| Sir Fraser Stoddart | 2016 | Chemistry | ↓ 年份降序      |
| Sir John Gurdon     | 2012 | Medicine  | 同2012年...   |
| Sir Martin Evans    | 2007 | Medicine  | ↓ 年份降序      |
### **IN逻辑表达式在条件排序中的使用**


- **核心思想：** 在 `ORDER BY` 子句中，利用 `CASE` 表达式为符合 `IN` 条件的行生成一个特定的值（比如 `0`），为不符合条件的行生成另一个值（比如 `1`）。然后首先按这个 `0/1` 值排序（升序），这样 `0`（符合条件的）就自然排在了 `1`（不符合条件的）前面。之后，你可以再按其他字段（如名称、ID等）进行二次排序。

- **为什么需要这个技巧？**
  默认情况下，`ORDER BY` 只是单纯地按照你指定的列升序或降序排列。它**不会**自动把 `WHERE IN (...)` 筛选出来的结果放在最前面。这个技巧就是为了**强制**让满足特定 `IN` 条件的结果集出现在查询结果的**顶部**。

- **语法和原理**

``` sql
SELECT 列1, 列2, ...
FROM 表名
WHERE ... -- 可选的过滤条件
ORDER BY
    CASE
        WHEN 目标列 IN (值1, 值2, 值3, ...) THEN 0 -- 符合条件的给 0
        ELSE 1 -- 不符合条件的给 1
    END, -- 首先按这个 0/1 标志排序 (升序 ASC 是默认的, 所以 0 在前)
    其他排序列1 [ASC|DESC], -- 然后按其他列排序
    其他排序列2 [ASC|DESC];
```

- **关键点解析**

1. **`CASE` 表达式：**
    
    - `WHEN 目标列 IN (...) THEN 0`： 检查目标列的值是否在你指定的值列表中。如果是，返回 `0`。
        
    - `ELSE 1`： 如果目标列的值**不在**指定的值列表中，返回 `1`。
        
    - 这个 `CASE` 表达式为每一行计算出一个**排序权重值** (`0` 或 `1`)。
        
2. **`ORDER BY` 子句：**
    
    - `ORDER BY CASE ... END, ...`： 排序的第一优先级就是这个计算出来的 `0/1` 权重值。
        
    - 因为 `ORDER BY` 默认是升序 (`ASC`)，所以 `0` 会排在 `1` 的前面。所有符合 `IN` 条件的行（权重 `0`）就会显示在最前面。
        
    - 在权重值排序之后，你可以再按其他需要的列（比如 `Name`, `ID`, `Date` 等）进行排序。这保证了在“符合条件的组”和“不符合条件的组”内部，数据也是有序的。

- **经典例题**

The expression **subject IN ('chemistry','physics')** can be used as a value - it will be **0** or **1**. Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last.

- 思路：

1. 使用表达式：`subject IN ('chemistry','physics')` 会为每条记录返回一个值：

- 如果subject是'chemistry'或'physics'，返回1（TRUE）

- 否则返回0（FALSE）

2. 在ORDER BY子句中，我们首先按这个0/1值排序（升序，因为默认ASC，所以0在前，1在后）。这样非化学物理的（0）就会排在前，化学物理的（1）排在后。

3. 然后，在同一个组内（比如都是0的组或都是1的组），再按subject（奖项名称）排序，最后按winner（获奖者姓名）排序。

``` sql
-- 示例：判断结果
SELECT 
  winner,
  subject
FROM nobel
WHERE yr = 1984
ORDER BY 
  subject IN ('chemistry','physics'), -- 先按0/1排序（0在前，1在后）
  subject, -- 然后按subject升序（A-Z）
  winner -- 最后按winner升序（A-Z）
```

- **结果示例：**

|winner|subject|
|----------|--------|
|Jaroslav Seifert|literature|
|César Milstein|medicine|
|GeorgesF. Köhler|medicine|
|Niels Jerne|medicine|
|Desmond Tutu|peace|
|Bruce Merrifield|chemistry|
|Carlo Rubbia|physics|
|Simon van der Meer|physics|

