## hbase 命令行操作
- 基本操作
```shell
# 进入
hbase shell

# 查看全部表, 随意一个字母<tab>查看命令
list

# 进入一张表， ta是一个引用
ta = get_table 'table_name'

# 操作表, 操作引用，扫描表
ta.scan 

# 记录个数
ta.count

# 查看全部命令
ta. <tab>键查看所有可以的命令

# 查看表信息
ta.describe

# 获取表的某一行
ta.get 'id'
ta.get 'id', 'c1','c2'
ta.get 'id', ['c1','c2']
ta.get 'id', {COLUMN => ['c1','c2']}

# 创建表
create 'table_name', {NAME => 'f1'， TTL => 秒}

# 添加元素
put 'table_name', '列', 'f1:id', "001"
```

- scan用法
```shell
# 查询表具体的列
scan 'table_name', {COLUMNS => ['f1:createTime']}

# 查询表具体的列并限制个数
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3}

# 过滤器：值过滤ValueFilter, RowKey过滤PrefixFilter, Column过滤ColumnPrefixFilter
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3, FILTER => "ValueFilter(=, 'binary:1')"}
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3, FILTER => "PrefixFilter('1') AND ValueFilter(=, 'binary:1')"}
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3, FILTER => "ColumnPrefixFilter('valid') AND ValueFilter(=, 'binary:1')"}  
```
> 过滤器参考： http://openinx.github.io/2019/07/02/blog-for-filter-list/


## 将文件中的数据导入到hbase表

- 场景
  
  在hbase中创建了一张新表，本地有一批数据，数据为纯数据(rowKey#v1#v2#v3)，将这批数据导入到表中
- 操作
```shell
# 建表
create 'table_name' {NAME => 'f1'}

# 文本文件拷贝到hdfs上，文件格式v1#v2#v3，例如3个字段name,age, id#ym#31
hadoop fs -put local_file /tmp/table_name

# map reduce任务
hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.separator="#" -Dimporttsv.columns=HBASE_ROW_KEY,f1:name,f1:age table_name /tmp/table_name
```

## 同步hbase表中数据
- 场景
  
  同步一张hbase表的数据到另外一张hbase表，覆盖操作
- 操作
```shell
step1: 获取hbase表对应的hdfs文件 /data/[org_table_name]/f1/*
step2: 将获取的文件放置要覆盖的hbase表的hdfs路径 hadoop fs -put [files] /data/[current_table_name]/f1/
step3: 进入HBaseShell界面, 清除表数据 truncate '[current_table_name]'
step4: 命令行操作 hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /data/[current_table_name] [current_table_name]
step5: 验证数据, 进入HBaseShell界面, count 'recsys_item_detail_longterm' 不为0为数据正常
```
