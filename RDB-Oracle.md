# 目次
- [型変換・フォーマット](#型変換・フォーマット)  
　・[TO_CHAR()](#tochar)  
　　- [数値型からの変換](#数値型からの変換tochar)  
- [目的別テクニック](#目的別テクニック)  
　・[階層問合せ](#階層問合せ)  
　　- [カンマ区切りの値を列に変換する](#カンマ区切りの値を列に変換する)  
　　- [カンマ区切りの連想配列風の値を行列に変換する](#カンマ区切りの連想配列風の値を行列に変換する)  
　・[和集合](#和集合)  
　　- [SQL上でバインド変数の値別に条件を変える](#SQL上でバインド変数の値別に条件を変える)
- [その他](#その他)  
　・[システム関連](#システム関連)  
　　- [Oracleのバージョン](#oracleのバージョン)  
　　- [Oracleの文字コード](#oracleの文字コード)  
　・[トラブルシュート](#トラブルシュート)  
　　- [Oracleの実行履歴](#oracleの実行履歴)  

# 型変換・フォーマット
## TO_CHAR()
### 数値型からの変換(TO_CHAR())
・フォーマット指定より桁数がオーバーフローすると###表記になる。  
・フォーマット指定より小数点以下がある場合は四捨五入される。  
```sql
-- 桁数指定で変換
TO_CHAR(数値, '999999.99')  -- 数値=1030.5 -> '  1030.5 '
TO_CHAR(数値, '000009.00')  -- 数値=1030.5 -> '001030.50'
-- カンマ区切り
TO_CHAR(数値, '999,999.99') -- 数値=1030.5 -> '  1,030.5 '
TO_CHAR(数値, '000,009.00') -- 数値=1030.5 -> '001,030.50'
```

# 目的別テクニック
## 階層問合せ
### カンマ区切りの値を列に変換する
**SQL** 
```sql
    SELECT TRIM(REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL)) AS "戻り値"
      FROM DUAL
CONNECT BY REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL) IS NOT NULL
```
**結果例**
・値 ＝ 'A,B,C'  
| 行 | 戻り値 | 
| - | - | 
| 1 | A | 
| 2 | B | 
| 3 | C | 

### カンマ区切りの連想配列風の値を行列に変換する
**SQL** 
```sql
SELECT TRIM(SUBSTR(TEST.戻り値, 1, INSTR(TEST.戻り値, ':', 1, 1) -1)) AS "戻り値１"
     , TRIM(SUBSTR(TEST.戻り値,    INSTR(TEST.戻り値, ':', 1, 1) +1)) AS "戻り値２"
  FROM(SELECT REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL) AS "戻り値"
         FROM DUAL
   CONNECT BY REGEXP_SUBSTR(値, '[^,]+', 1, LEVEL) IS NOT NULL
      )TEST
```
**結果例**  
・値 ＝ 'A:111,B:222,C:333'  
| 行 | 戻り値１ | 戻り値２ |
| - | - | - | 
| 1 | A | 111 | 
| 2 | B | 222 | 
| 3 | C | 333 | 

## 和集合
### SQL上でバインド変数の値別に条件を変える
```sql
AND EXISTS(SELECT 1  FROM DUAL WHERE '0' = NVL(:値, '0')
        UNION ALL
           SELECT 1  FROM DUAL WHERE '1' = :値 AND 比較対象１  = 比較対象２
        UNION ALL
           SELECT 1  FROM DUAL WHERE '2' = :値 AND 比較対象１ <> 比較対象２
          )
```
AND条件にそれぞれのバインド変数の条件と、かつUNION ALLでまとめることで、バインド変数の値が異なる毎に一致する条件を変えることができる。

# その他
## システム関連
### Oracleのバージョン
```sql
SELECT * FROM V$VERSION
```
### Oracleの文字コード
```sql
SELECT VALUE
  FROM NLS_DATABASE_PARAMETERS
 WHERE PARAMETER = 'NLS_CHARACTERSET'
```
## トラブルシュート
### Oracleの実行履歴
```sql
--SQLSTATS
  SELECT LAST_ACTIVE_TIME                     AS "問合せ最終時刻"
       , SQL_ID
       , PLAN_HASH_VALUE
       , CPU_TIME / POWER(10, 3)              AS "CPU時間(ミリ秒)"
       , ELAPSED_TIME / POWER(10, 3)          AS "解析-実行-フェッチ時間(ミリ秒)"
       , APPLICATION_WAIT_TIME / POWER(10, 3) AS "アプリ待機時間(ミリ秒)"
       , CLUSTER_WAIT_TIME / POWER(10, 3)     AS "クラスタ待機時間(ミリ秒)"
       , USER_IO_WAIT_TIME / POWER(10, 3)     AS "I/O待機時間(ミリ秒)"
       , SORTS                                AS "子カーソル合計ソート回数"
       , EXECUTIONS                           AS "子カーソル合計実行数"
       , SQL_TEXT                             AS "SQL文"
    FROM V$SQLSTATS
   WHERE LAST_ACTIVE_TIME  BETWEEN TO_DATE('開始日付', 'YYYYMMDDHH24MI')
                               AND TO_DATE('終了日付', 'YYYYMMDDHH24MI')
ORDER BY LAST_ACTIVE_TIME DESC

--SQLAREA
  SELECT LAST_ACTIVE_TIME                     AS "問合せ最終時刻"
       , PARSING_SCHEMA_NAME                  AS "スキーマ"
       , "MODULE"                             AS "実行元"
       , SQL_ID
       , PLAN_HASH_VALUE
       , CPU_TIME / POWER(10, 3)              AS "CPU時間(ミリ秒)"
       , ELAPSED_TIME / POWER(10, 3)          AS "解析-実行-フェッチ時間(ミリ秒)"
       , APPLICATION_WAIT_TIME / POWER(10, 3) AS "アプリ待機時間(ミリ秒)"
       , CLUSTER_WAIT_TIME / POWER(10, 3)     AS "クラスタ待機時間(ミリ秒)"
       , USER_IO_WAIT_TIME / POWER(10, 3)     AS "I/O待機時間(ミリ秒)"
       , SORTS                                AS "子カーソル合計ソート回数"
       , EXECUTIONS                           AS "子カーソル合計実行数"
       , SQL_TEXT                             AS "SQL文"
    FROM V$SQLAREA
   WHERE PARSING_SCHEMA_NAME NOT IN ('SYS', 'SYSMAN', 'DBSNMP', 'MDSYS', 'EXFSYS')
     AND LAST_ACTIVE_TIME   BETWEEN TO_DATE('開始日付', 'YYYYMMDDHH24MI')
                                AND TO_DATE('終了日付', 'YYYYMMDDHH24MI')
ORDER BY LAST_ACTIVE_TIME DESC
```