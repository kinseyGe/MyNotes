**以下我们在Centos7操作系统上以zookeeper-3.5.5为例来进行一下安装**

# 环境准备
> http://archive.apache.org/dist/zookeeper/zookeeper-3.5.5/apache-zookeeper-3.5.5-bin.tar.gz

> Jdk1.8 64位

> bigdata用户

# 开始安装

1. 将下载好的安装包上传至/home/bigdata/packages目录下

2. 进入到/home/bigdata/packages执行解压命令

```
tar -zxvf apache-zookeeper-3.5.5-bin.tar.gz -C ../
```

3. 配置zoo.cfg，如果没有此文件则拷贝zoo_simple.cfg为zoo.cfg

4. 编辑zoo.cfg

```
修改数据目录
dataDir=/home/bigdata/data/zookeeper/data

新增日志目录
dataLogDir=/home/bigdata/data/zookeeper/logs
```

5. 进入到解压目录下的bin目录下编辑zkEnv.sh

```
if [ "x${ZOO_LOG_DIR}" = "x" ]
then
    ZOO_LOG_DIR="/home/bigdata/data/zookeeper/logs"
    #ZOO_LOG_DIR="$ZOOKEEPER_PREFIX/logs"
fi
```

6. 新建日志目录和数据目录

```
mkdir /home/bigdata/data/zookeeper/{data,logs} -p
```

7. 启动zookeeper，进入到解压目录下的bin目录下

```
./zkServer.sh start
```
查看zk状态

![zk状态](http://tva1.sinaimg.cn/large/007X8olVly1g8idxd7ouhj30n103kwf9.jpg)
