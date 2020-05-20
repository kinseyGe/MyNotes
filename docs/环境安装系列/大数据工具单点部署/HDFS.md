**以下我们在Centos7操作系统上以hadoop-3.1.3为例来进行一下安装**

# 环境准备

> http://archive.apache.org/dist/hadoop/core/hadoop-2.6.1/hadoop-2.6.1.tar.gz

> Jdk1.8 64位

> bigdata用户

# 开始安装

1. 将下载好的安装包上传至/home/bigdata/packages目录下

2. 进入到/home/bigdata/packages执行解压命令

```
tar -zxvf hadoop-2.6.1.tar.gz -C ../
```

## 配置Hadoop

1. 进入到解压目录下的etc/hadoop目录下

2. 配置环境变量修改当前用户下的.bashrc

```
export HADOOP_HOME=/home/bigdata/hadoop-2.6.1
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native
export HADOOP_USER_NAME=bigdata
export JAVA_HOME=/home/bigdata/jdk1.8.0_161
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$JAVA_HOME/bin
```

编辑完成后执行生效命令

```
source .bashrc
```

3. 编辑core-site.xml文件

```
<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://risen:9000</value>
	</property>
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/home/bigdata/data/hadoop</value>
	</property>
</configuration>
```
保存并退出

4. 编辑hdfs-site.xml文件

```
<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
</configuration>
```

5. 编辑mapred-site.xml文件

> cp mapred-site.xml.template mapred-site.xml

```
<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
	<property>
		<name>mapreduce.map.memory.mb</name>
		<value>2048</value>
	</property>
	<property>
		<name>mapreduce.map.java.opts</name>
		<value>-Xmx2048M</value>
	</property>
	<property>
　		<name>mapreduce.reduce.memory.mb</name>
		<value>2048</value>
	</property>
	<property>
		<name>mapreduce.reduce.java.opts</name>
		<value>-Xmx2048M</value>
	</property>
</configuration>
```

6. 编辑hadoop-env.sh

```
export HADOOP_PREFIX=/home/bigdata/hadoop-2.6.1
```

7. 编辑slaves,修改为自己的主机名

```
risen
```

7. 配置免秘钥此处略过，查看linux常用命令文档

8. 格式化文件系统

```
hdfs namenode –format
```

9. 启动，进入到解压目录下sbin目录下执行命令

```
./start-all.sh
```

9. 启动没有报错，查看一下进程jps

![进程结果](http://tva1.sinaimg.cn/large/007X8olVly1g8id4pk2yfj308905774r.jpg)

9. 查看web页面

![50070 Web页面](http://tva1.sinaimg.cn/large/007X8olVly1g8imyoltaqj316c0mijv4.jpg)
