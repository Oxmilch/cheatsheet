## **標準関数** 

## **階層問合せ** 
### **カンマ区切りの値を列に変換する** 
#### **SQL** 
```sql
SELECT     TRIM(REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL)) AS "戻り値"
FROM       DUAL
CONNECT BY REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL) IS NOT NULL
```
#### **結果例**
・値 ＝ 'A,B,C'  
| 行 | 戻り値 | 
| - | - | 
| 1 | A | 
| 2 | B | 
| 3 | C | 

### **カンマ区切りの連想配列風の値を行列に変換する** 
#### **SQL** 
```sql
SELECT TRIM(SUBSTR(TEST.戻り値, 1, INSTR(TEST.戻り値, ':', 1, 1) -1)) AS "戻り値１"
     , TRIM(SUBSTR(TEST.戻り値,    INSTR(TEST.戻り値, ':', 1, 1) +1)) AS "戻り値２"
FROM  (SELECT     REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL) AS "戻り値"
       FROM       DUAL
       CONNECT BY REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL) IS NOT NULL
      )TEST
```
#### **結果例**
・値 ＝ 'A:111,B:222,C:333'  
| 行 | 戻り値１ | 戻り値２ |
| - | - | - | 
| 1 | A | 111 | 
| 2 | B | 222 | 
| 3 | C | 333 | 

## **テクニック** 
### **SQL上でバインド変数の値別に条件を変える** 
```sql
AND EXISTS(SELECT 1  FROM DUAL WHERE '0' = NVL(:値, '0')
           UNION ALL
           SELECT 1  FROM DUAL WHERE '1' = :値 AND 比較対象１  = 比較対象２
           UNION ALL
           SELECT 1  FROM DUAL WHERE '2' = :値 AND 比較対象１ <> 比較対象２
          )
```
AND条件にそれぞれのバインド変数の条件と、かつUNION ALLでまとめることで、  
バインド変数の値が異なる毎に一致する条件を変えることができる。