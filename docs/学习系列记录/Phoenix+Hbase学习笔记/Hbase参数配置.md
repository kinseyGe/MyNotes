- hbase.rootdir

> HBase集群中所有RegionServer共享目录，用来持久化HBase的数据，一般设置的是hdfs的文件目录

```
<property>
   <name>hbase.rootdir</name>
   <value>hdfs://risen:9000/hbase</value>
</property>
```

- hbase.tmp.dir

> 本地文件系统tmp目录，一般配置成local模式的设置一下，但是最好还是需要设置一下，因为很多文件都会默认设置成它下面的

```
<property>
   <name>hbase.tmp.dir</name>
   <value>/home/bigdata/data/hbase/tmp</value>
</property>
```

- hbase.zookeeper.property.dataDir

> ZooKeeper的zoo.conf中的配置。 快照的存储位置

```
<property>
   <name>hbase.zookeeper.property.dataDir</name>
   <value>/home/bigdata/data/hbase/zk</value>
</property>
```

- hbase.zookeeper.quorum

> zookeeper集群的URL配置，多个host中间用逗号（,）分割

```
<property>
   <name>hbase.zookeeper.quorum</name>
   <value>risen</value>
</property>
```

- hbase.master.maxclockskew

> 增大容忍度，默认是30s，但不要太大，毕竟时间不一致是不正常现象，可将所有节点和同一服务器时间做同步，也可以和时间服务器同步。

```
<property>
   <name>hbase.master.maxclockskew</name>
   <value>150000</value>
</property>
```

- hbase.master.info.port

> hbase master的web ui页面的端口

```
<property>
   <name>hbase.master.info.port</name>
   <value>60010</value>
</property>
```

- hbase.cluster.distributed

> 集群的模式，分布式还是单机模式，如果设置成false的话，HBase进程和Zookeeper进程在同一个JVM进程。

```
<property>
   <name>hbase.cluster.distributed</name>
   <value>true</value>
</property>
```

- hbase.unsafe.stream.capability.enforce

> 完全分布式式必须为false

```
<property>
   <name>hbase.unsafe.stream.capability.enforce</name>
   <value>false</value>
</property>
```

- hbase.table.sanity.checks

> hbase的各种参数检查

```
<property>
   <name>hbase.table.sanity.checks</name>
   <value>false</value>
</property>
```

- hbase.rpc.timeout

> 该参数表示一次RPC请求的超时时间。如果某次RPC时间超过该值，客户端就会主动关闭socket,默认为秒

```
<property>
   <name>hbase.rpc.timeout</name>
   <value>1200000</value>
</property>
```

- hbase.client.operation.timeout

> 该参数表示HBase客户端发起一次数据操作直至得到响应之间总的超时时间，数据操作类型包括get、append、increment、delete、put等。该值与hbase.rpc.timeout的区别为,hbase.rpc.timeout为一次rpc调用的超时时间。而hbase.client.operation.timeout为一次操作总的时间(从开始调用到重试n次之后失败的总时间)

```
<property>
   <name>hbase.client.operation.timeout</name>
   <value>600000</value>
</property>
```

- hbase.client.scanner.timeout.period

> 该参数是表示HBase客户端发起一次scan操作的rpc调用至得到响应之间总的超时时间。一次scan操作是指发起一次regionserver rpc调用的操作,hbase会根据scan查询条件的cacheing、batch设置将scan操作会分成多次rpc操作。比如满足scan条件的rowkey数量为10000个，scan查询的cacheing=200，则查询所有的结果需要执行的rpc调用次数为50个。而该值是指50个rpc调用的单个相应时间的最大值。

```
<property>
   <name>hbase.client.scanner.timeout.period</name>
   <value>1200000</value>
</property>
```

- hbase.regionserver.lease.period

> 客户端租用HRegion server 期限，即超时阀值

```
<property>
   <name>hbase.regionserver.lease.period</name>
   <value>1200000</value>
</property>
```

- hbase.client.ipc.pool.type

> socket连接池类型

```
<property>
   <name>hbase.client.ipc.pool.type</name>
   <value>RoundRobinPool</value>
</property>
```

- hbase.client.ipc.pool.size

> socket连接池大小

```
<property>
   <name>hbase.client.ipc.pool.size</name>
   <value>10</value>
</property>
```
