**以下我们在Centos7操作系统上以RocketMQ4.3.1为例来进行一下安装**

# 集群部署模式说明

## 单master

> 这种方式风险较大，一旦Broker重启或者宕机时，会导致整个服务不可用，不建议线上环境使用

## 多master模式

- 一个集群无Slave，全是Master，例如2个Master或者3个Master。

> 优点：配置简单，单个Master宕机或重启维护对应用无影响，在磁盘配置为RAID10时，即使机器宕机不可恢复情况下，由于RAID10磁盘非常可靠，消息也不会丢失（异步刷盘丢失少量消息，同步刷盘一条不丢）。性能最高。

> 缺点：单台机器宕机期间，这台机器上未被消费的消息在机器恢复之前不可订阅，消息实时性会受到影响。

## 多Master多Slave模式，异步复制

- 每个Master配置一个Slave，有多对Master-Slave，HA采用异步复制方式，主备有短暂消息延迟，毫秒级。

> 优点：即使磁盘损坏，消息丢失的非常少，且消息实时性不会受影响，因为Master 宕机后，消费者仍然可以从Slave消费，此过程对应用透明。不需要人工干预。性能同多 Master 模式几乎一样。

> 缺点：Master宕机，磁盘损坏情况，会丢失少量消息。

## 多Master多Slave模式，同步双写

- 每个Master配置一个Slave，有多对Master-Slave，HA采用同步双写方式，主备都写成功，向应用才返回成功。

> 优点：数据与服务都无单点，Master宕机情况下，消息无延迟，服务可用性与数据可用性都非常高。

> 缺点：性能比异步复制模式略低，大约低10%左右，发送单个消息的RT会略高。目前主宕机后，备机不能自动切换为主机，后续会支持自动切换功能。

# 选定模式

> 为了保证我们消息的完整性和可靠性，我们采用多master多slave同步双写的方式搭建。其他模式可根据实际业务场景搭建，万变不离其宗。

# 环境准备
> 下载地址 http://rocketmq.apache.org/release_notes/release-notes-4.3.1/

> Maven环境(如果不安装可视化插件，此步骤可省略)

> 可视化插件下载地址 https://github.com/apache/rocketmq-externals/releases/tag/rocketmq-console-1.0.0

- 分配节点信息

IP地址 | 主机名
---|---
192.168.5.57 | risen01
192.168.5.58 | risen02
192.168.5.59 | risen03

- 分配角色信息

IP地址 | 角色(端口)
---|---
192.168.5.57 | Master0(10911)+slave1(10920)
192.168.5.58 | Master0(10911)+slave1(10920)
192.168.5.59 | slave2(10920)

- 注意：++涉及到分布式安装，如果有过分布式安装经验的同学，通俗的来讲配置文件多数大部分是一样的，所以我们可以巧妙的shell脚本或者scp等命令，而无需像下面一样一个配置文件一个配置文件的来配置，还好下面只是三台机器，如果多加几台，那么工作量无疑会很大。++

# 操作用户

>root


# 开始安装

## 解压rocketmq压缩包

> 操作节点：192.168.5.57

> 操作用户：root

1. 将安装包通过sftp或者客户端工具或者rz命令等方式上传至/usr/local目录下

2. 在/usr/local目录下执行解压命令并重命名为rocketmq-4.3.1

	```
	unzip rocketmq-all-4.3.1-bin-release.zip
	
	mv rocketmq-all-4.3.1-bin-release rocketmq-4.3.1
	```
	
    ![RocketMQ解压](https://ftp.bmp.ovh/imgs/2019/09/078e88589707ce9d.png)

## 修改jvm参数配置

> 操作节点：192.168.5.57

> 操作用户：root

1. 进入到rocketmq-4.3.1目录下的bin目录下,编辑nameserver服务需要的启动文件runserver.sh

    > JVM Configuration第一行修改为下面

    ```
    #====================================================================
    # JVM Configuration
    #====================================================================
    JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx4g -Xmn512m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
    保存并退出(此参数配置根据实际服务器硬件条件调大或调小)
    ```

2. 进入到rocketmq-4.3.1目录下的bin目录下，编辑broker服务需要的启动文件runbroker.sh

    > JVM Configuration第一行修改为下面
    
    ```
    JAVA_OPT="${JAVA_OPT} -server -Xms2g -Xmx4g -Xmn1g"
    保存并退出(此参数配置根据实际服务器硬件条件调大或调小)
    ```

## 配置环境变量

> 操作节点：192.168.5.57

> 操作用户：root

1. 添加ROCKETMQ_HOME环境变量,修改/etc/profile并新增

    ```
    export ROCKETMQ_HOME=/usr/local/rocketmq-4.3.1
    export PATH=$PATH:$ROCKETMQ_HOME/bin
    ```
    
## 配置文件介绍

> 操作节点：192.168.5.57

> 操作用户：root

1. 进入到rocketmq-4.3.1目录下的conf目录下如下图

    ![配置文件介绍](https://ftp.bmp.ovh/imgs/2019/09/45e7af5e2421158e.png) 
    
    - 2m-2s-async 目录为双master双slave异步复制配置目录
    
    - 2m-2s-sync 目录为双master双slave同步双写配置目录
    
    - 2m-noslave 目录为多master配置目录
    
    > 这里我们只需要配置2m-2s-sync这个目录即可。
    
## 配置第一个主节点

> 操作节点：192.168.5.57

> 操作用户：root

1. 进入到/usr/local/rocketmq-4.3.1/conf/2m-2s-sync目录下,编辑broker-a.properties配置文件(以下配置信息如果已存在则可以跳过)：

    ```
    #所属集群名字
    brokerClusterName=rocketmq_cluster
    #通过配置文件指定机器的集群和broker归属，比如brokerName一样的属于同一个broker,一个broker只能有一个master，可以有多个slave，brokerId为0的为master，其他的为slave。
    brokerName=broker-a
    #0表示Master，>0表示Slave
    brokerId=0
    #删除文件时间点，默认凌晨 4点
    deleteWhen=04
    #文件保留时间，默认 48 小时
    fileReservedTime=48
    #Broker 的角色
    brokerRole=SYNC_MASTER
    #异步刷新ASYNC_FLUSH /同步刷新SYNC_FLUSH
    flushDiskType=SYNC_FLUSH
    
    #nameServer地址，分号分割
    namesrvAddr=192.168.5.57:9876;192.168.5.58:9876
    #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
    defaultTopicQueueNums=4
    #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
    autoCreateTopicEnable=true
    #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
    autoCreateSubscriptionGroup=true
    #Broker 对外服务的监听端口
    listenPort=10911
    #commitLog每个文件的大小默认1G
    mapedFileSizeCommitLog=1073741824
    #ConsumeQueue每个文件默认存30W条，根据业务情况调整
    mapedFileSizeConsumeQueue=300000
    #检测物理文件磁盘空间
    diskMaxUsedSpaceRatio=88
    #存储路径
    storePathRootDir=/data/rocketmq-m
    #commitLog 存储路径
    storePathCommitLog=/data/rocketmq-m/commitlog
    #消费队列存储路径存储路径
    storePathConsumeQueue=/data/rocketmq-m/consumequeue
    #消息索引存储路径
    storePathIndex=/data/rocketmq-m/index
    #checkpoint 文件存储路径
    storeCheckpoint=/data/rocketmq-m/checkpoint
    #abort 文件存储路径
    abortFile=/data/rocketmq-m/abort
    #限制的消息大小
    maxMessageSize=65536
    ```

2. 创建存储路径执行命令
    
    ```
    mkdir /data/rocketmq-m/{commitlog,consumequeue,index,checkpoint,abort} -p
    ```

## 启动第一个主节点

> 操作节点：192.168.5.57

> 操作用户：root

1. 启动第一个主节点服务(进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令)

    ```
    mqnamesrv
    ```
    ![启动第一个主节点服务](https://ftp.bmp.ovh/imgs/2019/09/49ca107c3038b9a1.png)
    
    出现以上启动信息说明nameserver服务启动成功。

2. 启动第一个主节点进程(进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令)
    
    ```
    ./mqbroker -c ../conf/2m-2s-sync/broker-a.properties
    ```
    ![启动第一个主节点进程](https://ftp.bmp.ovh/imgs/2019/09/f9762601f4fe45de.png)
    
    出现以上启动信息说明nameserver进程启动成功。

3. 检查启动进程

    ![启动进程](https://ftp.bmp.ovh/imgs/2019/09/b47f115c89b23d43.png)
    
    出现以上启动信息更加验证了master启动成功。

## 配置第一个子节点

> 操作节点：192.168.5.57

> 操作用户：root

1. 进入到/usr/local/rocketmq-4.3.1/conf/2m-2s-sync目录下编辑broker-a-s.properties配置文件(以下配置信息如果已存在则可以跳过)

    ```
    brokerClusterName=rocketmq_cluster
    brokerName=broker-a
    brokerId=1
    deleteWhen=04
    fileReservedTime=48
    brokerRole=SLAVE
    flushDiskType=SYNC_FLUSH
    #nameServer地址，分号分割
    namesrvAddr=192.168.5.57:9876;192.168.5.58:9876
    #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
    defaultTopicQueueNums=4
    #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
    autoCreateTopicEnable=true
    #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
    autoCreateSubscriptionGroup=true
    #Broker 对外服务的监听端口
    listenPort=10920
    #commitLog每个文件的大小默认1G
    mapedFileSizeCommitLog=1073741824
    #ConsumeQueue每个文件默认存30W条，根据业务情况调整
    mapedFileSizeConsumeQueue=300000
    #检测物理文件磁盘空间
    diskMaxUsedSpaceRatio=88
    #commitLog 存储路径
    storePathCommitLog=/data/rocketmq-s/commitlog
    #消费队列存储路径存储路径
    storePathConsumeQueue=/data/rocketmq-s/consumequeue
    #消息索引存储路径
    storePathIndex=/data/rocketmq-s/index
    #checkpoint 文件存储路径
    storeCheckpoint=/data/rocketmq-s/checkpoint
    #abort 文件存储路径
    abortFile=/data/rocketmq-s/abort
    #限制的消息大小
    maxMessageSize=65536
    ```

2. 创建存储路径，执行命令

```
mkdir /data/rocketmq-s/{commitlog,consumequeue,index,checkpoint,abort} -p
```

## 启动第一个子节点

> 操作节点：192.168.5.57

> 操作用户：root

1. 启动主节点进程(进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令)

    ```
    ./mqbroker -c ../conf/2m-2s-sync/broker-a-s.properties
    ```
    ![启动第一个子节点](https://ftp.bmp.ovh/imgs/2019/09/54ee9f2d0695ae30.png)
    
    出现以上启动信息说明nameserver进程启动成功。

## 配置第二个主节点

> 操作节点：192.168.5.58

> 操作用户：root

1. 按照上面同样的配置，配置好jvm参数
2. 进入到/usr/local/rocketmq-4.3.1/conf/2m-2s-sync目录下，编辑broker-b.properties配置文件(以下配置信息如果已存在则可以跳过)

    ```
    brokerClusterName=rocketmq_cluster
    brokerName=broker-b
    brokerId=0
    deleteWhen=04
    fileReservedTime=48
    brokerRole=SYNC_MASTER
    flushDiskType=SYNC_FLUSH
    #nameServer地址，分号分割
    namesrvAddr=192.168.5.57:9876;192.168.5.58:9876
    #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
    defaultTopicQueueNums=4
    #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
    autoCreateTopicEnable=true
    #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
    autoCreateSubscriptionGroup=true
    #Broker 对外服务的监听端口
    listenPort=10911
    #commitLog每个文件的大小默认1G
    mapedFileSizeCommitLog=1073741824
    #ConsumeQueue每个文件默认存30W条，根据业务情况调整
    mapedFileSizeConsumeQueue=300000
    #检测物理文件磁盘空间
    diskMaxUsedSpaceRatio=88
    #存储路径
    storePathRootDir=/data/rocketmq-m
    #commitLog 存储路径
    storePathCommitLog=/data/rocketmq-m/commitlog
    #消费队列存储路径存储路径
    storePathConsumeQueue=/data/rocketmq-m/consumequeue
    #消息索引存储路径
    storePathIndex=/data/rocketmq-m/index
    #checkpoint 文件存储路径
    storeCheckpoint=/data/rocketmq-m/checkpoint
    #abort 文件存储路径
    abortFile=/data/rocketmq-m/abort
    #限制的消息大小
    maxMessageSize=65536
    ```

3. 创建存储路径执行命令

    ```
    mkdir /data/rocketmq-m/{commitlog,consumequeue,index,checkpoint,abort} -p
    ```

## 启动第二个主节点

> 操作节点：192.168.5.58

> 操作用户：root

1. 启动主节点服务(进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令)

    ```
    ./mqnamesrv
    ```
    
 ![启动第二个主节点服务](https://ftp.bmp.ovh/imgs/2019/09/cbdf754068de64e7.png)

    出现以上启动信息说明nameserver服务启动成功。

2. 启动第二个主节点进程(进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令)

    ```
    ./mqbroker -c ../conf/2m-2s-sync/broker-b.properties
    ```
    ![启动第二个主节点进程](https://ftp.bmp.ovh/imgs/2019/09/f4d44b9ee8309c74.png)
    
    出现以上启动信息说明nameserver进程启动成功
    
## 配置第二个子节点

> 操作节点：192.168.5.58

> 操作用户：root

1. 进入到/usr/local/rocketmq-4.3.1/conf/2m-2s-sync目录下，编辑broker-b-s.properties配置文件(以下配置信息如果已存在则可以跳过)

    ```
    brokerClusterName=rocketmq_cluster
    brokerName=broker-b
    brokerId=1
    deleteWhen=04
    fileReservedTime=48
    brokerRole=SLAVE
    flushDiskType=SYNC_FLUSH
    #nameServer地址，分号分割
    namesrvAddr=192.168.5.57:9876;192.168.5.58:9876
    #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
    defaultTopicQueueNums=4
    #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
    autoCreateTopicEnable=true
    #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
    autoCreateSubscriptionGroup=true
    #Broker 对外服务的监听端口
    listenPort=10920
    #commitLog每个文件的大小默认1G
    mapedFileSizeCommitLog=1073741824
    #ConsumeQueue每个文件默认存30W条，根据业务情况调整
    mapedFileSizeConsumeQueue=300000
    #检测物理文件磁盘空间
    diskMaxUsedSpaceRatio=88
    #commitLog 存储路径
    storePathCommitLog=/data/rocketmq-s/commitlog
    #消费队列存储路径存储路径
    storePathConsumeQueue=/data/rocketmq-s/consumequeue
    #消息索引存储路径
    storePathIndex=/data/rocketmq-s/index
    #checkpoint 文件存储路径
    storeCheckpoint=/data/rocketmq-s/checkpoint
    #abort 文件存储路径
    abortFile=/data/rocketmq-s/abort
    #限制的消息大小
    maxMessageSize=65536
    ```

2. 创建存储路径，执行命令

    ```
    mkdir /data/rocketmq-s/{commitlog,consumequeue,index,checkpoint,abort} -p
    ```

## 启动第二个子节点

> 操作节点：192.168.5.58

> 操作用户：root

1. 启动主节点进程(进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令)

    ```
    ./mqbroker -c ../conf/2m-2s-sync/broker-b-s.properties
    ```
    ![启动第二个子节点](https://ftp.bmp.ovh/imgs/2019/09/4112d83a713eaf68.png)

    出现以上启动信息说明nameserver进程启动成功

## 配置第三个子节点

> 操作节点：192.168.5.59

> 操作用户：root

1. 进入到/usr/local/rocketmq-4.3.1/conf/2m-2s-sync目录下，执行命令
    
    ```
    cp broker-b-s.properties broker-c-s.properties
    ```

2. 编辑broker-c-s.properties文件

    ```
    brokerClusterName=rocketmq_cluster
    brokerName=broker-b
    brokerId=2
    deleteWhen=04
    fileReservedTime=48
    brokerRole=SLAVE
    flushDiskType=SYNC_FLUSH
    #nameServer地址，分号分割
    namesrvAddr=192.168.5.57:9876;192.168.5.58:9876
    #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
    defaultTopicQueueNums=4
    #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
    autoCreateTopicEnable=true
    #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
    autoCreateSubscriptionGroup=true
    #Broker 对外服务的监听端口
    listenPort=10920
    #commitLog每个文件的大小默认1G
    mapedFileSizeCommitLog=1073741824
    #ConsumeQueue每个文件默认存30W条，根据业务情况调整
    mapedFileSizeConsumeQueue=300000
    #检测物理文件磁盘空间
    diskMaxUsedSpaceRatio=88
    #commitLog 存储路径
    storePathCommitLog=/data/rocketmq-s/commitlog
    #消费队列存储路径存储路径
    storePathConsumeQueue=/data/rocketmq-s/consumequeue
    #消息索引存储路径
    storePathIndex=/data/rocketmq-s/index
    #checkpoint 文件存储路径
    storeCheckpoint=/data/rocketmq-s/checkpoint
    #abort 文件存储路径
    abortFile=/data/rocketmq-s/abort
    #限制的消息大小
    maxMessageSize=65536
    ```

3. 创建存储路径，执行命令

    ```
    mkdir /data/rocketmq-s/{commitlog,consumequeue,index,checkpoint，abort} -p
    ```

## 启动第三个子节点

> 操作节点：192.168.5.59

> 操作用户：root

1. 启动主节点进程，进入到/usr/local/rocketmq-4.3.1/bin目录下执行命令

    ```
    ./mqbroker -c ../conf/2m-2s-sync/broker-c-s.properties
    ```
    ![image](https://ftp.bmp.ovh/imgs/2019/09/4906ecd17eb0147a.png)


# 完成搭建
> 截止到这一步，整个集群的搭建就算是完成了。上述所有涉及到的执行命令，如果启动无误，均可后台执行(后台自行百度命令nohub，这里不再阐述)


# 安装可视化插件

> 操作系统Windows

1. 我已经将上面可视化插件放在的D盘目录下，进入到D:\rocketmq-externals-rocketmq-console-1.0.0\rocketmq-console\src\main\resources目录下修改application.properties文件:

    ![可视化插件配置](http://tva1.sinaimg.cn/large/007X8olVly1g6wlj59dslj313x08oq3e.jpg)

    - 修改为紫色框中的配置即可，其中
    
        - server.port为可视化插件的端口，
        - rocketmq.config.namesrvAddr为rocketmq集群的主节点配置
        - rocketmq.config.dataPath为插件的日志目录

2. 编译可视化插件，将下载好的可视化插件，解压后，通过cmd进行编译，执行命令

    ```
    mvn clean package -Dmaven.test.skip=true
    ```
    ![Maven编译](http://tva1.sinaimg.cn/large/007X8olVly1g6wlmh15sxj30ih0hnt90.jpg)
    
    出现以上信息则说明编译打包成功，此时会在该目录下生成一个target目录，我们只需要里面的rocketmq-console-ng-1.0.0.jar这个文件
    
3. 启动可视化插件

> 操作节点：192.168.5.59

> 操作用户：root

- 将rocketmq-console-ng-1.0.0.jar上传至/usr/local/rocketmq-4.3.1目录下，然后执行命令

    ```
    java -jar rocketmq-console-ng-1.0.0.jar
    ```
    ![启动可视化插件](http://tva1.sinaimg.cn/large/007X8olVly1g6wmkpd8lpj30zb05a3yw.jpg)
    
    即可启动可视化插件，出现以上信息说明启动成功。然后我们通过端口访问一下: http://192.168.5.59:18080
    
    ![可视化插件展示](http://tva1.sinaimg.cn/large/007X8olVly1g6wmltwahvj318d0dpdh8.jpg)
    
    出现以上界面，则证明没有问题，可以清楚的看到整个集群的节点角色信息以及版本和消息，主题的使用情况等。无误后，我们可以将java -jar 后台执行。

# 问题总结

- Rocketmq4.3.1需要的jdk为1.8，如果环境中已存在其他版本jdk，那么该怎么办呢？

    ![问题一](http://tva1.sinaimg.cn/large/007X8olVly1g6wmnimd3sj30mp029weg.jpg)
    
    Metaspacesize为jdk1.8新特性，所以此时我们需要单独为Rocketmq4.3.1指定jdk，只需要向runserver.sh和runbroker.sh文件中添加一句
    
    ```
    export JAVA_HOME=jdk1.8的安装目录
    ```