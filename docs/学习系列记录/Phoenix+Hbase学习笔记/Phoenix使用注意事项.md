# Phoenix使用注意事项以及跟标准sql的不同

> 参考链接：https://www.jianshu.com/p/53021d046987

- CHAR类型只能保存单字节的字符，不能保存中文字符，中文需要使用VARCHAR

- 非主键字段不能指定为NOT NULL，不支持指定字段默认值(4.9版本以上支持)。

- UPDATE table SET col=val WHERE：不支持该语法，phoenix更新和插入使用同样的UPSERT INTO语法，如果主键存在则更新，不存在则插入。但可以使用UPSERT INTO table SELECT语句来实现条件更新，需要把主键按条件选出来，如：

  ```
  UPSERT INTO table(id, col1) SELECT id, 'val1' FROM table WHERE col2 = val2
  ```
- DATE/TIME字段类型：这两个字段类型对应java.sql.Date类型，其JDBC驱动不会对java.util.Date类型进行转换，所以在PreparedStatement中setObject(int, java.util.Date)会报下面的错误。如果使用jfinal ActiveRecord保存实体，当实体字段是java.util.Date类型时要注意。

- LIMIT OFFSET分页：phoenix 4.6不支持使用OFFSET分页，在4.8以上版本才支持。

- ALTER不支持改变表名、字段名与字段类型，只能增加与删除字段。在设计的时候要注意使用合适的字段类型。

- 大小写：phoenix默认不区分大小写，所有表名、列名都是大写。

- 不支持的函数：IFNULL, ISNULL等；IFNULL有对等的COALESCE()函数

- 连接池：phoenix不推荐使用连接池，因为其基于HBase的连接的创建成本很低，并且用过的HBase连接不能共享使用，因此用过的Connection需要关闭

- JDBC Connection: AutoCommit默认为false，其他JDBC Driver一般默认是true，需要手动条用connnection.commit()或者在连接url后面加上";autocommit=true"

- 索引使用：使用CREATE INDEX idx创建的是全局索引(GLOBAL INDEX)，还有一种本地索引(LOCAL INDEX)，相对于本地索引，全局索引可以让读的性能更佳，但写入的时候成本会高一点；而本地索引在写入的时候成本较低，但读的时候成本较高。使用全局索引的时候，如果SELECT字段含有非索引的字段，则索引不会被使用，大多数情况下我们SELECT都会有索引之外的字段，这时候需要考虑建立覆盖索引(CREATE INDEX INCLUDING ...)或使用hint来让phoenix使用索引：SELECT /*+ INDEX(table idx) */ col FROM table WHERE ...。

- PreparedStatement不支持Statement.RETURN_GENERATED_KEYS，getGeneratedKeys()。

- 日期类型保存为UTC时间，在shell/squirrel查询的时候要用CONVERT_TZ做转换，CONVERT_TZ(col, 'UTC', 'Asia/Shanghai')，但有个bug，如果col字段是timestamp的话，会报Type mismatch. expected: [DATE]。实在需要转换的话先转成Date类型：CONVERT_TZ(TO_DATE(TO_CHAR(col,'yyyy-MM-dd HH:mm:ss'),'yyyy-MM-dd HH:mm:ss'),'UTC','Asia/Shanghai')

# 关于Phoenix的一些优化

- 主键：主键对应HBase的row key，HBase会把相近的row key放到相同的region里，如果选择的主键是单调递增的，那么某个region就会变成热点，写入的性能会变差，这种情况需要在建表的使用SALT_BUCKETS=N来自动对数据分片。

- 优化读：使用全局索引，这会对写入的速度有一定影响。查询时注意使用exlain来看执行计划是否使用了索引。

- 优化写：使用本地索引，建表时预先指定好分区方案(pre-split)。

- 数据是否可变：如果数据只是写入，以后不会变更，可以在建表的时候指定IMMUTABLE_ROWS=true，这样可以提高写入的性能。

- 适当使用查询hint来优化执行计划：用explain查看执行计划，用hint来优化。支持的语法见hinting。
