使用 XGBoost 外部存储器版本（测试版）
===========================================
使用外部存储器版本和内存版本没有太大的区别。
唯一的区别是文件名格式。

外部存储器版本采用以下文件名格式
```
filename#cacheprefix
```

```filename``` 是要加载的 libsvm 文件的正常路径， ```cacheprefix``` 是一个 xgboost 将用于外部内存缓存的缓存文件的路径。

下面的代码是从 [../demo/guide-python/external_memory.py](../demo/guide-python/external_memory.py) 中提取的。
```python
dtrain = xgb.DMatrix('../data/agaricus.txt.train#dtrain.cache')
```
你可以在 libsvm 文件后面找到 ```#dtrain.cache``` ，这是缓存文件的名称。
对于 CLI 版本，只需在文件名中使用 ```"../data/agaricus.txt.train#dtrain.cache"``` 。

性能说明
----------------
* 参数 ```nthread``` 应该设置为 ***real*** cores 的数量
  - 大多数现代 CPU 提供 hyperthreading （超线程），这意味着你可以拥有 8 个线程的 4 核 CPU
  - 在这种情况下，将 nthread 设置为 4 以获得更高性能

分布式版本
-------------------
外部存储器模式很自然地适用于分布式版本，您可以简单地设置路径就像这样
```
data = "hdfs:///path-to-data/#dtrain.cache"
```
xgboost 会将数据缓存到本地位置。在 YARN 上运行时，当前文件夹是暂时的，这样你就可以直接使用 ```dtrain.cache``` 来缓存当前文件夹。


用法说明
----------------------
* 这是一个实验版本
  - 如果你想尝试测试，请将结果报告至 https://github.com/dmlc/xgboost/issues/244
* 目前只支持从 libsvm 格式导入
  - 欢迎从其他常见的外部存储器数据源采集的贡献
