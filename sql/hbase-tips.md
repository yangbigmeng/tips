## hbase 命名行操作
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
```
# 查询表具体的列
scan 'table_name', {COLUMNS => ['f1:createTime']}

# 查询表具体的列并限制个数
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3}

# 过滤器：值过滤ValueFilter, RowKey过滤PrefixFilter, Column过滤ColumnPrefixFilter
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3, FILTER => "ValueFilter(=, 'binary:1')"}
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3, FILTER => "PrefixFilter('1') AND ValueFilter(=, 'binary:1')"}
scan 'table_name', {COLUMNS => ['f1:valid'], LIMIT => 3, FILTER => "ColumnPrefixFilter('valid') AND ValueFilter(=, 'binary:1')"}  
```
