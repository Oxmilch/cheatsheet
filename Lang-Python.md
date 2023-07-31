## 参考サイト
- [logger - Pythonのloggingをスッキリ使いこなす 自調自考の旅](https://own-search-and-study.xyz/2019/10/20/python-logging-clear/)

```python
# loggerは残り続けるため削除する処理が必要
# loggerからハンドラーの削除
for handle in handles:
    logger.removeHandler(handle)
＃ loggerの削除
del logging.Logger.manager.loggerDict[logger.name]
```