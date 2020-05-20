- 问题描述: Hbase启动报错
> Please check the config value of 'hbase.procedure.store.wal.use.hsync' to set the desired level of robustness and ensure the config value of 'hbase.wal.dir' points to a FileSystem mount that can provide it

![Hbase启动报错](http://tva1.sinaimg.cn/large/007X8olVly1g8ichids53j317e09lgry.jpg)

**解决方案**

```
在hbase-site.xml中新增以下配置，重启服务
<property>
  <name>hbase.unsafe.stream.capability.enforce</name>
  <value>false</value>
</property>
```
---

- 问题描述: Phoenix或其它客户端连接HBASE报错
>org.apache.hadoop.hbase.DoNotRetryIOException: Class org.apache.phoenix.coprocessor.MetaDataEndpointImpl cannot be loaded Set hbase.table.sanity.checks to false at conf or table descriptor if you want to bypass sanity checks

![连接Hbase报错](http://tva1.sinaimg.cn/large/007X8olVly1g8ihuirxycj311l0653zy.jpg)

**解决方案**

```
在hbase-site.xml中新增以下配置，重启服务
<property>
    <name>hbase.table.sanity.checks</name>
    <value>false</value>
</property>
```
---

- 问题描述: hbase shell报错

> nhandled Java exception: java.lang.IncompatibleClassChangeError: Found class jline.Terminal, but interface was expected
java.lang.IncompatibleClassChangeError: Found class jline.Terminal, but interface was expected

![hbase shell报错](http://tva1.sinaimg.cn/large/007X8olVly1g8lz4tyumvj317p0gmqav.jpg)

**解决方案**

> 这是由于jline的jar包版本太低，所以需要换成高版本

> 下载链接：https://repo1.maven.org/maven2/jline/jline/2.12/jline-2.12.jar 替换掉/home/bigdata/hadoop-2.6.1/share/hadoop/yarn/lib下的jline-0.9.94.jar即可
