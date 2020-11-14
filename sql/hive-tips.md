## 关键字

|关键词|描述|
|----|----|
|order by |排序, 一个reducer，先选定全部query查询，再全局排序|
|group by|分组, group by字段select需包含|
|sort by| desc/asc使用, 根据类型排序, reduce之前排序，每个reducer输出有序 |
| cluster by | distribute by 和sort by的另一种方式，多个reducer，分组，分组间有序|
| distribute by | 分组, 所有相同分组进入同一个reducer, 多个reducer之间不重复|
| row_number() over(...) | 安排序号, 组内连续唯一|
| select distinct| 去重, 可指定字段名称|
| select join | 主键和外键的满足条件的合集 |
| left outer join | left table 全部行, 不匹配补充null |
| right outer join | right table 全部行, 不匹配补充null |
| full outer join | 2个表全部, 不匹配补充null |
