---
title: SQL Learning Note_5
published: 2025-07-01T23:51:57+08:00
summary: CASE WHEN
cover:
  image: https://i1.hdslb.com/bfs/archive/88f8d10085625622d4c4a774dc4d773bd1205966.jpg
tags:
  - Notes
categories: Learning Journeys
draft: false
lang:
---
{{< netease_music "5233900" >}}

### 一、CASE WHEN 基础

#### 1. 作用

- 实现条件逻辑（类似编程语言中的`if-else`）

- 在SELECT、WHERE、ORDER BY等子句中使用

#### 2. 两种语法形式

**(1) 简单CASE表达式** → 等值比较

```sql

CASE 列名

WHEN 值1 THEN 结果1

WHEN 值2 THEN 结果2

...

ELSE 默认结果

END

```

**(2) 搜索CASE表达式** → 复杂条件

```sql

CASE

WHEN 条件1 THEN 结果1

WHEN 条件2 THEN 结果2

...

ELSE 默认结果

END

```

### 二、核心功能详解

#### 1. 在SELECT中创建新列

```sql

SELECT name,

CASE WHEN population > 100000000 THEN '大国'

WHEN population > 50000000 THEN '中等国'

ELSE '小国'

END AS country_size

FROM world

```

#### 2. 在WHERE中条件过滤

```sql

SELECT *

FROM orders

WHERE

CASE

WHEN status = '紧急' THEN NOW() - order_date < INTERVAL '1 day'

ELSE NOW() - order_date < INTERVAL '7 days'

END

```

#### 3. 在ORDER BY中自定义排序

```sql

SELECT winner, subject

FROM nobel

ORDER BY

CASE WHEN subject IN ('Chemistry','Physics') THEN 1 ELSE 0 END, -- 化学物理排最后

subject

```

#### 4. 在GROUP BY中条件聚合

```sql

SELECT

SUM(CASE WHEN subject = 'Physics' THEN 1 ELSE 0 END) AS physics_count,

AVG(CASE WHEN yr >= 2000 THEN prize_amount END) AS avg_modern_prize

FROM nobel

```

### 三、高级技巧

#### 1. 嵌套CASE

```sql

CASE

WHEN score > 90 THEN 'A'

WHEN score > 80 THEN

CASE WHEN bonus = 1 THEN 'B+' ELSE 'B' END

...

END

```

#### 2. 与聚合函数结合

```sql

-- 行列转换（PIVOT）

SELECT

yr,

COUNT(CASE WHEN subject = 'Physics' THEN 1 END) AS physics,

COUNT(CASE WHEN subject = 'Chemistry' THEN 1 END) AS chemistry

FROM nobel

GROUP BY yr

```

#### 3. 处理NULL值

```sql

SELECT name,

CASE WHEN address IS NULL THEN '未知' ELSE address END AS safe_address

FROM users

```

### 四、经典案例：奖项分类

#### 题目要求

> 将诺贝尔奖分为三类显示：

> - 科学奖（物理、化学、医学）→ 标记为 `Science`

> - 文学与和平奖 → 标记为 `Arts`

> - 其他 → 标记为 `Other`

#### 解决方案

```sql

SELECT winner, subject,

CASE

WHEN subject IN ('Physics','Chemistry','Medicine') THEN 'Science'

WHEN subject IN ('Literature','Peace') THEN 'Arts'

ELSE 'Other'

END AS category

FROM nobel

```

#### 结果示例

| winner           | subject    | category |
| ---------------- | ---------- | -------- |
| Albert Einstein  | Physics    | Science  |
| Marie Curie      | Chemistry  | Science  |
| Ernest Hemingway | Literature | Arts     |