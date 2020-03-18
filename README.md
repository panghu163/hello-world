
# SST导入导出
SST导入导出工具适用于DCDB的SST文件的备份与还原
## 目录结构
![Alt text](./SST-1.png)
- SSTBackup.py：主程序
- ./conf/gflags：配置文件
- ./Backup：SST保存的目录（配置文件可配）
- init_data.json：SST文件信息（第一次备份前不会产生此文件）
## 配置文件
![Alt text](./SST-2.png)
> 所有配置选项和参数必须使用`" "`，并且注意结尾的`,`最后一个选项不用以`，`结尾。

- "sst_path"：SST文件将要保存的目录（目录必须以`/`结尾）
- "ingest"：在导入入SST时，是否导入最新的SST
- "meta_item"：meta集群中任一meta节点的IP+PORT
- "disk_limit"：配置SST不可导出的最大磁盘剩余率。图中的含义是磁盘剩余小于10%，SST不再导出到sst_path下

## store配置
在store的配置文件中，配置`-socket_max_unwritten_bytes=536870912`选项。

## 命令格式
![Alt text](./SST-4.png)



## 统计信息
可以统计当前的导出情况，首次导出前，由于没有SST，所以不显示统计信息。
#### 命令：
``` python
python SSTBackup.py localinfo
```

![Alt text](./SST-3.png)
- Table ：当前导出SST的表
- RegionID：Table对应的RegionID
- CreateTime：Table创建的时间
- LastBackupTime：最后导出的时间
- LogIndex：最后导出的log index
- FileSize：导出region的size
## 导出
导出功能用于备份SST，可以全部导出和表导出。
> 导出的文件为最新的SST文件。
### 全部导出
#### 命令
``` python
python SSTBackup.py download all
```
### 表导出
#### 命令
```python
python SSTBackup.py download table_name
```

## 导入
导入功能用于把导出的SST还原到DCDB中，导入功能分为全部导入和表导入。
### 全部导入
#### 命令
```python
python SSTBackup.py upload all
```
### 表导入
#### 命令
``` python
python SSTBackup.py upload table_name
```




`
