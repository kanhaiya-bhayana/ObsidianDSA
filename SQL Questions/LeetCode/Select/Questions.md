## 1. [Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)

```sql
-- Write your PostgreSQL query statement below

SELECT  product_id
FROM    Products
WHERE   low_fats='Y' AND recyclable='Y';
```
## 2. [Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)

```sql
-- Write your PostgreSQL query statement below

SELECT  name
FROM    Customer
WHERE   referee_id != 2 OR referee_id IS NULL;
```
## 3. [Big Countries](https://leetcode.com/problems/big-countries/)

```sql
-- Write your PostgreSQL query statement below

SELECT  w.name, w.population, w.area
FROM    World w
WHERE   w.area >= 3000000 OR w.population >= 25000000;
```

## 4. [Article Views I](https://leetcode.com/problems/article-views-i/)

```sql
-- Write your PostgreSQL query statement below

SELECT      DISTINCT author_id AS id
FROM        Views
WHERE       author_id = viewer_id
ORDER BY    id  ASC;    
```

## [Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)

```sql
-- Write your PostgreSQL query statement below

SELECT      tweet_id
FROM        Tweets
WHERE       LENGTH(content) > 15;
```
