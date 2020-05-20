**以下我们在Centos7操作系统上以hbase-2.0.0为例来进行一下安装**

# 环境准备
> https://mirrors.tuna.tsinghua.edu.cn/apache/hbase/2.0.0/hbase-2.0.0-bin.tar.gz

> Jdk1.8 64位

> bigdata用户

# 开始安装

1. 将下载好的安装包上传至/home/bigdata/packages目录下

2. 进入到/home/bigdata/packages执行解压命令

  ```
  tar -zxvf hbase-2.0.0-bin.tar.gz -C ../
  ```

## 配置Hbase

1. 进入到conf目录下编辑文件hbase-env.sh

```
# 不是用hbase自带的zk
export HBASE_MANAGES_ZK=false

# 配置jdk home
export JAVA_HOME=/home/bigdata/jdk1.8.0_161
```

2. 编辑hbase-site.xml文件

```
<configuration>
	<property>
		<name>hbase.rootdir</name>
		<value>hdfs://risen:9000/hbase</value>
	</property>
	<property>
		<name>hbase.tmp.dir</name>
		<value>/home/bigdata/data/hbase/tmp</value>
	</property>
	<property>
		<name>hbase.zookeeper.property.dataDir</name>
		<value>/home/bigdata/data/hbase/zk</value>
	</property>
	<property>
		<name>hbase.zookeeper.quorum</name>
		<value>risen</value>
	</property>
	<property>
		<name>hbase.master.maxclockskew</name>
		<value>150000</value>
	</property>
	<property>
		<name>hbase.master.info.port</name>
		<value>60010</value>
	</property>
	<property>
		<name>hbase.cluster.distributed</name>
		<value>true</value>
	</property>
	<property>
		<name>hbase.unsafe.stream.capability.enforce</name>
		<value>false</value>
	</property>
	<property>
    <!--hbase的各种参数检查-->
    <name>hbase.table.sanity.checks</name>
		<value>false</value>
	</property>
	<property>
		<name>phoenix.schema.isNamespaceMappingEnabled</name>
		<value>true</value>
	</property>
</configuration>
```
3. 创建以上配置中填写的目录

```
mkdir -r -p /home/bigdata/data/hbase/{tmp,zk}
```

4. 进入到bin目录下启动

```
./start-hbase.sh
```
![hbase shell](http://tva1.sinaimg.cn/large/007X8olVly1g8ie33o563j315z0ddae4.jpg)

查看web页面

![Hbase 60010 web页面](http://tva1.sinaimg.cn/large/007X8olVly1g8ie4rdk8dj318e0mr77f.jpg)

5. 修改~/.bashrc文件，添加到环境变量

```
export HBASE_HOME=/home/bigdata/hbase-2.0.0
export PATH=$PATH:$HBASE_HOME/bin
```
使环境变量生效
```
source ~/.bashrc
```
