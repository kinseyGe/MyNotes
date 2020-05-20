**以下我们在Centos7操作系统上以phoenix-5.0.0-HBase-2.0为例来进行一下安装**

# 环境准备

> http://mirrors.tuna.tsinghua.edu.cn/apache/phoenix/apache-phoenix-5.0.0-HBase-2.0/bin/apache-phoenix-5.0.0-HBase-2.0-bin.tar.gz

> Jdk1.8 64位

> HBase的环境变量需要配置

> bigdata用户

# 开始安装

1. 将下载好的安装包上传至/home/bigdata/packages目录下

2. 进入到/home/bigdata/packages执行解压命令

  ```
  tar -zxvf apache-phoenix-5.0.0-HBase-2.0-bin.tar.gz -C ../
  ```

## 开始配置

1. 将解压目录下phoenix-5.0.0-HBase-2.0-server.jar复制到hbase的安装目录下的lib目录下

2. 进入到bin目录下执行连接命令

```
./sqlline.py 192.168.10.49:2181
```
![Phoenix连接](http://tva1.sinaimg.cn/large/007X8olVly1g8in4pzrqwj31710jh45q.jpg)

## 配置事务

1. 需要分别将一下配置加入到Phoenix安装目录下的bin下的hbase-site.xml以及hbase安装目录下的conf目录下的hbase-site.xml文件中

```
<!-- 开启phoenix事务支持 -->
<property>
        <name>phoenix.transactions.enabled</name>
        <value>true</value>
</property>
<!-- 存储tx状态的快照目录 -->
<property>
        <name>data.tx.snapshot.dir</name>
        <value>/tmp/tephra/snapshots</value>
</property>
<!-- 事务超时时间 -->
<property>
        <name>data.tx.timeout</name>
        <value>60</value>
</property>
```

2. 启动事务服务，进入到Phoenix安装目录下执行命令

```
./tephra
```
> 注意：如果遇到报错可以查看[**常见问题总结**]目录

![查看事务服务状态](http://tva1.sinaimg.cn/large/007X8olVly1g8lrxsa349j30bb02674d.jpg)

![事务演示](http://tva1.sinaimg.cn/large/007X8olVly1g8lsk3kpepj31670i6jvc.jpg)
