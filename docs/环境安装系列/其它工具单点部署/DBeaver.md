**DBeaver EE是一个通用的数据库管理工具和 SQL 客户端，支持 MySQL, PostgreSQL, Oracle, DB2, MSSQL, Sybase, Mimer, HSQLDB, Derby, 以及其他兼容 JDBC 的数据库。**

# 环境准备

> 下载地址：https://pan.baidu.com/s/1F9ydGGn7FeUAub1FjtR3Iw  提取码：otta

> 安装版本：6.0.6 这个版本相对来说比较高了

> 如果链接失效可以联系作者

# 开始安装

> 这才是重点，没有破解的可以连接关系型数据库以及其他的个别数据库，破解的是真的强，nosql的数据库也可以连接，非结构化的亦是如此。

1. 这里直接按照正常Windows程序安装即可，这里省略跳过啦

2. 配置破解，可以查看压缩包中的readme.txt

## 连接示例

1. 连接Phoenix(我这里用到的是phoenix-5.0.0-HBase-2.0)

  - 首先将phoenix-5.0.0-HBase-2.0-client.jar的包从服务器上拉下来，放到桌面或者任意盘符

  - 新建Phoenix连接，配置主机名，Phoenix连接的是zk的2181端口，所以这里端口写2181

    - 点击**编辑驱动设置**

    - 点击**添加文件**,选择刚才我们从服务器上下载下来的客户端驱动包

  - 点击测试连接即可连接成功
  - (如果连接时报错org.apache.hadoop.security.GroupMappingServiceProvider此相关信息，可能是之前dbeaver下载了其他版本的依赖，需要进入到dbeaver的本地maven库删除org.apache.phoenix目录即可)

2. 连接DBeaver没有的数据库类型

- 点击**菜单栏**中的**数据库**

  - 点击**驱动管理器**

    - 点击**新建**

      - 这里我们以国产数据库金仓8为例来创建一个金仓8的连接
        ![金仓8配置驱动连接](http://tva1.sinaimg.cn/large/007X8olVly1g8lumamj1cj30e80fb0ub.jpg)
      - 保存关闭

- 右键**新建连接**

  ![新建金仓连接](http://tva1.sinaimg.cn/large/007X8olVly1g8lupxw02jj30hf0g8wfx.jpg)

- 填入主机名，用户名，密码即可，其他地方根据实际情况修改

> 其实说白了，上图中的连接跟我们通过代码用jdbc/odbc连接是一样的。
