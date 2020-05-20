**以下我们在银河麒麟操作系统上以filebeat为例来进行一下安装**

![国产服务器信息](https://s1.ax1x.com/2020/05/12/YYX2OU.jpg)

# 环境准备
> beats https://codeload.github.com/elastic/beats/zip/v5.4.3

> git
```
  1. 配置好/etc/apt/sources.list源
  2. apt-get install git
```
> go https://studygolang.com/dl/golang/go1.14.2.linux-arm64.tar.gz

> 联网环境

# 开始安装
1. 上传beats-master.zip 到服务器上并解压
2. 进入到解压目录下的file-beat目录中执行`make`命令
3. 等待一会即可交叉编译成功
4. 将编译好的文件重新打成tar.gz包 `tar -zcvf filebeat.tar.gz filebeat/*`
5. 最终要的是对于环境受限的朋友可以直接下载此交叉编译后的包
> 链接：https://pan.baidu.com/s/17t3Q4Dv9caubordZW26eKQ 提取码：wzc5
