**以下我们在Centos7操作系统上以redis为例来进行一下安装**

# 环境准备
> 下载地址 http://download.redis.io/releases/redis-3.2.1.tar.gz

> Redis 要求集群至少6个节点,版本为5.0.0
如果为低版本可能会出现ruby版本过低的情况

![redis问题](https://ftp.bmp.ovh/imgs/2019/09/58f506e7aa859a51.png)

# 操作用户
>root


# 开始安装
1. 将安装包通过sftp或者客户端工具或者rz命令等方式上传至/root目录下
2. 执行解压命令并解压到/usr/local目录下

    ```
    tar -zxvf redis-3.2.1.tar.gz -C /usr/local
    ```
    
3. 进入到解压目录执行编译命令(编译环境的安装这里不再赘述)
    
    ```
    make
    ```

4. 在redis的解压目录中执行命令创建6个Redis配置文件

    ```
    mkdir {6379,6380,6381,6382,6383,6384}
    
    touch {6379/redis.conf,6380/redis.conf,6381/redis.conf,6382/redis.conf,6383/redis.conf,6384/redis.conf}
    ```
5. 分别配置配置文件的内容为(**不同端口的配置文件配置不同的端口**)

    ```
    daemonize    yes                          //redis后台运行
    pidfile  /var/run/redis_6379.pid
    port  6379
    cluster-enabled  yes                      //开启集群  把注释#去掉
    cluster-config-file  nodes_6379.conf      //集群的配置  配置文件首次启动自动生成 
    cluster-node-timeout  5000                //请求超时  设置5秒够了
    appendonly  yes                           //aof日志开启  有需要就开启，
    ```
    
    > 它每次写操作都会记录一条日志其中 port 和 pidfile 需要随着 文件夹的不同递增。

6. 进入到redis的解压目录，执行命令启动各个节点

    ```
    ./src/redis-server /usr/local/redis-5.0.0/6379/redis.conf
    ./src/redis-server /usr/local/redis-5.0.0/6380/redis.conf 
    ./src/redis-server /usr/local/redis-5.0.0/6381/redis.conf 
    ./src/redis-server /usr/local/redis-5.0.0/6382/redis.conf 
    ./src/redis-server /usr/local/redis-5.0.0/6383/redis.conf 
    ./src/redis-server /usr/local/redis-5.0.0/6384/redis.conf 
    ```

7. 检查各个节点redis进程

	![查看redis进程](https://ftp.bmp.ovh/imgs/2019/09/e54a71992abfa2d4.png)

8. 创建集群客户端

    ```
    ./src/redis-cli --cluster create 192.168.5.57:6379 192.168.5.57:6380 192.168.5.57:6381 192.168.5.57:6382 192.168.5.57:6383 192.168.5.57:6384 --cluster-replicas 1
    ```
    ![redis创建客户端集群](https://ftp.bmp.ovh/imgs/2019/09/378f01b0057c3354.png)
    ![redis创建客户端集群](https://ftp.bmp.ovh/imgs/2019/09/25007ab137e3af83.png)

	出现上图说明启动成功，可以看到三台master三台slave !

9. 关闭集群这里不在赘述，暴力一点直接杀进程，其他方式可以参考一下链接 https://my.oschina.net/ruoli/blog/2252393 