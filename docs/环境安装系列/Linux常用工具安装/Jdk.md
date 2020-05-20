**以下我们在Centos7操作系统上以oracle jdk1.7为例来进行一下安装**

# 环境准备
> https://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u79-oth-JPR

# 操作用户
>root

# 开始安装
1. 将安装包通过sftp或者客户端工具或者rz命令等方式上传至/root目录下
2. 执行解压命令并解压到/usr/local目录下

	```
	tar -zxvf jdk-7u79-linux-i586.tar.gz -C /usr/local
	```

3. 配置系统环境变量，使用vi或者vim编辑器编辑/etc/profile文件，并在最后一行加入以下内容：

	```
	export JAVA_HOME=/usr/local/jdk1.7.0_79
	export JAVA_BIN=$JAVA_HOME/bin
	export PATH=$PATH:$JAVA_HOME/bin
	export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	export JAVA_HOME JAVA_BIN PATH CLASSPATH
	```

4. 使环境变量生效，执行命令

	```
	source /etc/profile
	```

5. 检查是否安装成功，执行命令

	```
	java -version
	```
	![java -version](https://ftp.bmp.ovh/imgs/2019/09/9808347afe7c987f.jpg)

	可以看出上面已经成功打印出了JDK的版本，安装成功。