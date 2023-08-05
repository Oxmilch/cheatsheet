## 参考サイト
- [並列・並行処理　concurrent.futures](https://qiita.com/simonritchie/items/1ce3914eb5444d2157ac#concurrentfutures)
- [共有メモリ　multiprocessing Value, Array](https://qiita.com/t_okkan/items/4127a87177ed2b2db148)
```python
import concurrent.futures
# マルチスレッド（並列処理）
# 共有メモリ(multiprocessing.Value, Array)を使用することができる。
# 同じプロセス内で、処理の待ち時間を利用して処理を切り替えながら動作する。
# 処理が重く、情報を共有処理する必要がない場合に使える。
with concurrent.futures.ThreadPoolExecutor(maxWorkers=1) as exe:
    exe.submit(別プロセスで動かしたい関数, 引数)

# マルチプロセス（並行処理）
# 共有メモリ(multiprocessing.Value, Array)は使用できない
# 別プロセスで動作できるが、プロセスを作成する処理が激重。
# 
with concurrent.futures.ProcessPoolExecutor(maxWorkers=1) as exe:
    exe.submit(別プロセスで動かしたい関数, 引数)
``` 

- [logger - Pythonのloggingをスッキリ使いこなす 自調自考の旅](https://own-search-and-study.xyz/2019/10/20/python-logging-clear/)
```python
# loggerは残り続けるため削除する処理が必要
# loggerからハンドラーの削除
for handle in handles:
    logger.removeHandler(handle)
＃ loggerの削除
del logging.Logger.manager.loggerDict[logger.name]
```