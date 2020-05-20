**以下我们在Windows操作系统上以RokcetMQ4.3.1为例来进行一下安装**

# 环境准备
> http://rocketmq.apache.org/release_notes/release-notes-4.3.1/

> Jdk1.8 64位

# 开始安装

1. 解压rocketmq-all-4.3.1-bin-release.zip

2. 启动nameserver
    >方式一：进入到rocketmq解压后安装目录下的bin目录下执行**mqnamesrv.cmd**出现以下启动页面,则表示启动nameserver 成功。（此窗口勿关闭）
    
    >方式二：通过cmd进入到到rocketmq解压后安装目录下的bin目录下执行命令：
    
    ```
    start mqnamesrv.cmd
    ```

    ![rocketmq-nameserver](https://ftp.bmp.ovh/imgs/2019/09/a0225695b2019d10.png)

3. 启动broker
    >方式一：进入到rocket mq解压后安装目录下的bin目录下执行mqbroker.cmd出现以下启动页面,则表示启动broker 成功。（此窗口勿关闭）

    >方式二：通过cmd进入到到rocketmq解压后安装目录下的bin目录下执行命令：
    
    ```
    start mqbroker.cmd -n 127.0.0.1:9876 autoCreateTopicEnable=true
    ```

    ![rocketmq-broker](https://ftp.bmp.ovh/imgs/2019/09/9de335414de4ec90.png)
    