**以下我们在Centos7操作系统上以Elasticsearch5.4.3为例来进行一下安装**

# 环境准备
> Elasticsearch https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.3.tar.gz

> Cerebro可视化插件 https://github.com/lmenezes/cerebro/releases

> Jdk1.8(已阐述)

# 开始安装
1. 将安装包通过sftp或者客户端工具或者rz命令等方式上传至/root目录下
2. 执行解压命令并解压到/usr/local目录下

```
tar -zxvf elasticsearch-5.4.3.tar.gz -C /usr/local/

tar -zxvf cerebro-0.7.3.tgz -C /usr/local/elasticsearch-5.4.3
```

3.配置Elasticsearch

>进入到/usr/local/elasticsearch-5.4.3/config配置elasticsearch.yml **(注意：es的配置文件要求严格遵守缩进和空格)**

```
#配置集群名称
cluster.name: RisenES
#配置节点名称
node.name: node-1
#配置数据目录
path.data: /data/es/data
#配置日志目录
path.logs: /data/es/logs
#配置ip地址
network.host: 192.168.30.58
#配置http访问端口
http.port: 9200
```

4. 创建Elasticsearch中配置的目录

```
mkdir /data/es/{data,logs} -p
```

5. 修改Elasticsearch的属主属组

> **注意：elasticsearch在版本1之后的某个版本，已经不再支持root启动，必须是非root用的其他用户启动，所以我们需要创建用户，并修改elasticsearch属主和属组**

```
#添加用户
useradd bigdata
#修改权限
chown -R bigdata.bigdata /usr/local/elasticsearch-5.4.3
chown -R bigdata.bigdata /data/es
```

6. 启动Elasticsearch,切换到上一步骤创建好的bigdata用户

```
su - bigdata
```

7. 进入到/usr/local/elasticsearch-5.4.3/bin目录下为启动脚本配置jdk1.8的环境修改elasticsearch文件添加以下命令

```
export JAVA_HOME=/usr/local/jdk1.8.0_161
```
![设置es启动环境](https://ftp.bmp.ovh/imgs/2019/09/6da6d6221276fc2f.jpg)

8. 然后保存退出后执行命令

```
./elasticsearch
```

![临时启动es](https://ftp.bmp.ovh/imgs/2019/09/641adfdd12a2c8c4.jpg)

出现以上截图说明启动成功，然后Ctrl+c结束执行，将其加入到后台运行,执行命令：

```
./elasticsearch -d
```

如果启动出现问题，请看【常见错误总结】

9. 启动cerebro可视化插件

> 进入到/usr/local/elasticsearch-5.4.3/cerebro-0.7.3/conf目录修改application.conf文件最后

![配置cerebro](https://ftp.bmp.ovh/imgs/2019/09/d1cd2a6d382f87d2.jpg)

**注意：修改其中的host和name以及设置插件的登录账号和密码**

>修改完后进入到/usr/local/elasticsearch-5.4.3/cerebro-0.7.3/bin目录(如果你的环境jdk版本低于1.8，请执行以下命令，否则跳过此步骤)

```
export JAVA_HOME=/usr/local/jdk1.8.0_161/
export PATH=$JAVA_HOME/bin:$PATH 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

10. 然后执行cerebro启动命令

```
./cerebro -Dhttp.port=9201 -Dhttp.address=192.168.23.1
```

![启动cerebro](https://ftp.bmp.ovh/imgs/2019/09/99a3b57617542116.jpg)

出现以上截图说明启动成功，然后访问页面 http://192.168.23.1:9201

![访问cerebro登录页面](https://ftp.bmp.ovh/imgs/2019/09/e78ef0bef6b8d227.jpg)

点击之前配置的集群名称**RisenEs**

![在cerebro上选择配置的es名称](https://ftp.bmp.ovh/imgs/2019/09/e78ef0bef6b8d227.jpg)

出现以下界面:
![cerebro展示页面](https://ftp.bmp.ovh/imgs/2019/09/846184e6712a5c96.jpg)

以上就是可视化界面的一个详情，这里就不做过多解释页面的功能了。

> 这里说一下es的三种颜色状态：

- 绿色——最健康的状态，代表所有的主分片和副本分片都可用； 

- 黄色——所有的主分片可用，但是部分副本分片不可用； 
    
- 红色——部分主分片不可用。（此时执行查询部分数据仍然可以查到，遇到这种情况，还是赶快解决比较好。