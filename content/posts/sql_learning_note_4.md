
https://sqlzoo.net/wiki/SUM_and_COUNT
### 🎯 WHERE和HAVING根本区别总结

| |`WHERE`|`HAVING`|
|-----------|---------|---|
|**作用对象**|原始数据行（分组前）|分组后的聚合结果（分组后）|
|**执行时机**|在 `GROUP BY` 之前执行|在 `GROUP BY` 之后执行|
|**可使用**|列名、普通函数|聚合函数（SUM, COUNT, AVG 等）|
|**不可用**|聚合函数|原始列名（除非在 GROUP BY 中）|
|**类比**|筛选原材料|筛选成品|
 ###  **经典例题**
 - #### Counting big countries in each continent
 For each **continent** show the **continent** and number of countries with populations of at least 10 million.
 ```sql
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent

```

| continent     | COUNT(name) |
| ------------- | ----------- |
| Africa        | 29          |
| Asia          | 26          |
| Caribbean     | 2           |
| Eurasia       | 1           |
| Europe        | 14          |
| North America | 4           |
| Oceania       | 1           |
| South America | 8           |

 - #### Counting big continents
List the continents that **have** a total population of at least 100 million.
 
 ```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

|continent|
|---|
|Africa|
|Asia|
|Eurasia|
|Europe|
|North America|
|South America|