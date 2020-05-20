- phoenix.query.timeoutMs

> 在客户机上查询超时间的毫秒数。默认为10分钟 600000

```
<property>
   <name>phoenix.query.timeoutMs</name>
   <value>1800000</value>
</property>
```

- phoenix.query.keepAliveMs

> 当线程的数量大于客户端线程池可执行的数量时，超时时间的毫秒数，默认为60秒 60000
```
<property>
   <name>phoenix.query.keepAliveMs</name>
   <value>600000</value>
</property>
```

- phoenix.query.threadPoolSize

> 线程池中的线程数，随着集群中的机器/CPU数量的增长，这个值应该增加       128

- phoenix.query.queueSize

> 限制队列最大数，线程不够往队列里面装，队列装满了就拒绝请求，默认大小是5000，如果是0就同步

```
<property>
   <name>phoenix.query.queueSize</name>
   <value>5000</value>
</property>
```

- phoenix.query.spoolThresholdBytes

> 任务并行执行完毕后返回值写到磁盘的阀值，单位为字节，默认值是20 mb 20971520

```
<property>
   <name>phoenix.query.spoolThresholdBytes</name>
   <value>20971520</value>
</property>
```

- phoenix.query.maxSpoolToDiskByte

- 任务执行失败写入磁盘的阀值，默认是1G 1024000000

```
<property>
   <name>phoenix.query.maxSpoolToDiskByte</name>
   <value>1024000000</value>
</property>
```

- phoenix.query.maxGlobalMemorySize

> Phoenix查询内存的最大值，默认不设置

- phoenix.query.maxGlobalMemoryWaitMs

> 无剩余内存时，等待剩余内存使用的时间，超时就抛出，默认是10秒

```
<property>
   <name>phoenix.query.maxGlobalMemoryWaitMs</name>
   <value>10</value>
</property>
```

- phoenix.mutate.maxSize

> 当客户端提交请求后每次处理数据的最大值，默认为50W

```
<property>
   <name>phoenix.mutate.maxSize</name>
   <value>500000</value>
</property>
```

- phoenix.query.useIndexes

> 检查索引是否用优化器来查询，默认为true

- phoenix.groupby.maxCacheSize

> Group by查询的数据缓存大小，默认为100MB

- phoenix.groupby.spillFiles

> 使用group by查询的时候，当数据超出100MB的时候写入本地磁盘文件的个数    2

- phoenix.coprocessor.maxNetaDataCacheSize

> 元数据最大的缓存值，默认是20MB
