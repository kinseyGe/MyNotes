- 问题描述:启动事务tephra报错：com/google/common/util/concurrent/Service$Liste

![事务服务启动报错](http://tva1.sinaimg.cn/large/007X8olVly1g8lrk98ay3j30xg08xn1g.jpg)

**解决方案**

需要提升guava的jar包版本

> 下载地址 https://repo1.maven.org/maven2/com/google/guava/guava/14.0/

下载下来，然后替换掉hbase安装目录下的lib目录下的原来的guava-版本.jar包再次启动./tephra启动即可

---

- 问题描述:启动报错: 找不到或无法加载主类 org.apache.tephra.TransactionServiceMain

![事务服务启动报错](http://tva1.sinaimg.cn/large/007X8olVly1g8lrmmzirxj30lv03b3zm.jpg)

**解决方案**

> 参考Hbase单点搭建，配置环境变量即可

---

- 问题描述: 创建表报错

![非主键字段不能指定为NOT NULL](http://tva1.sinaimg.cn/large/007X8olVly1g8m5hzqqvej30oc0bgwfn.jpg)

**解决方案**

> 非主键字段不能指定为NOT NULL，不支持指定字段默认值(4.9版本以上支持)。

- 问题描述: 查询hbase超时

![Phoenix查询hbase超时](http://tva1.sinaimg.cn/large/007X8olVly1g8n9kzzrefj317k0hhqd3.jpg)

**解决方案**

> 修改Phoenix安装目录下的bin目录下的hbase-site.xml文件

```
<property>
    <name>phoenix.query.timeoutMs</name>
    <value>1800000</value>
</property>    
<property>
    <name>hbase.regionserver.lease.period</name>
    <value>1200000</value>
</property>
<property>
    <name>hbase.rpc.timeout</name>
    <value>1200000</value>
</property>
<property>
    <name>hbase.client.scanner.caching</name>
    <value>1000</value>
</property>
<property>
    <name>hbase.client.scanner.timeout.period</name>
    <value>1200000</value>
</property>
```
---
