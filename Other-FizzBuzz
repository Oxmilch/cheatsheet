# 色んな言語でFizzBuzz
## Oracle
### 普通に書く
```sql
 SELECT CASE WHEN MOD(LEVEL,15) = 0 THEN 'FizzBuzz'
             WHEN MOD(LEVEL,5)  = 0 THEN 'Buzz'
             WHEN MOD(LEVEL,3)  = 0 THEN 'Fizz'
             ELSE TO_CHAR(LEVEL)
        END AS  FIZZBUZZ
   FROM DUAL
CONNECT LEVEL <= 100
```
### SQLぽさを出して書いてみる
```sql
WITH TBL AS (
 SELECT LEVEL AS NUM
   FROM DUAL
CONNECT LEVEL <= 100  --1～100まで
)
,    MST AS (
   SELECT 3      AS NUM
        , 'Fizz' AS TXT
     FROM DUAL
UNION ALL
   SELECT 5      AS NUM
        , 'Buzz' AS TXT
     FROM DUAL
)
   -- 3と5で両方割れるときは2行になるので、１行にまとめつつFizzBuzzとなるように文字列結合をする。
   SELECT NVL(LISTAGG(MST.TXT) WITHIN GROUP(ORDER BY MST.NUM), TBL.NUM) AS FIZZBUZZ
     FROM TBL
LEFT JOIN MST
          --3で割ったとき、5で割ったときの余りが0の時に結合
       ON MOD(TBL.NUM, MST.NUM) = 0
 GROUP BY TBL.NUM
 ORDER BY TBL.NUM  ASC
```