**以下我们在Centos7操作系统上以nginx1.1.10为例来进行一下安装**

# 环境准备
> http://nginx.org/download/nginx-1.1.10.tar.gz

# 操作用户
>root

# 开始安装
1. 将安装包通过sftp或者客户端工具或者rz命令等方式上传至/root目录下
2. 执行解压命令并解压到/usr/local目录下

	```
	tar -zxvf nginx-1.1.10.tar.gz -C /usr/local
	```

3. 安装编译环境以及其他依赖(如果不存在)

	```
	yum -y install gcc automake autoconf libtool make gcc gcc-c++ openssl openssl-devel
	```

4. 进入到nginx解压路径，执行命令

	```
	./configure --prefix=/usr/local/nginx
	```

5. 执行编译命令和安装命令

	```
	make && make install
	```

6. 进入到安装目录/usr/local/nginx下并启动nginx

	```
	cd /usr/local/nginx/bin

	./nginx
	```

	> 访问地址 http://localhost:80 出现以下截图即为安装成功

	![nginx启动成功](https://ftp.bmp.ovh/imgs/2019/09/d5a3a33f53d7d11d.jpg)
