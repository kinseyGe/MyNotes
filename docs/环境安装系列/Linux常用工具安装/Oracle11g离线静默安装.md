**以下我们在Centos7操作系统上以oracle11g为例来进行一下安装**

# 环境准备
> Oracle下载地址 https://www.oracle.com/technetwork/database/enterprise-edition/downloads/112010-linx8664soft-100572.html

> JDK1.7(这里不再阐述)

> 可用的yum源(这里不再阐述)

# 操作用户
>root

# 开始安装

## 基础环境配置

> 操作节点：192.168.22.1

> 操作用户：root

1. 修改主机名IP映射关系，编辑/etc/hosts文件

    ```
    192.168.22.1 risen
    ```

2. 修改主机名，编辑/etc/hostname文件
    
    ```
    risen
    ```

3. 修改系统名称，编辑文件/etc/redhat-release并修改为redhat-7 (此步骤可跳过，后续安装如果不报错可不修改)

## 安装必备的依赖环境

1. 安装环境依赖
    > 操作节点：192.168.22.1

    > 操作用户：root
    
    ```
    yum -y install  binutils-* compat-libstdc++-* compat-libcap1-* \
    elfutils-libelf-* elfutils-libelf-devel-* gcc* gcc-c++-* glibc* \
    glibc-common-* glibc-devel-* glibc-headers-* ksh-* \
    libaio-* libaio-devel-* libgcc-* libstdc++-* libstdc++-devel* make-* \
    sysstat-* unixODBC-* unixODBC-devel-* numactl-devel-* pdksh-* \
    kernel-headers*
    ```

## 创建oracle相关用户以及用户组
    
> 操作节点：192.168.22.1

> 操作用户：root

```
groupadd oinstall
groupadd dba
useradd -g oinstall -G dba oracle
```

## 为oracle用户设置密码

> 操作节点：192.168.22.1

> 操作用户：root

```
echo "oracle" | passwd --stdin oracle
```

## 修改oracle环境变量

> 编辑/home/oracle/.bash_profile文件，并新增以下内容

    > 操作节点：192.168.22.1
    
    > 操作用户：root

    ```
    umask 022
    stty erase ^H
    PATH=$PATH:$HOME/bin
    TMP=/tmp
    TMPDIR=$TMP
    ORACLE_BASE=/home/oracle/app/oracle
    ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
    ORACLE_SID=orcl
    ORACLE_TERM=xterm
    PATH=$PATH:$HOME/bin:$ORACLE_HOME/bin
    LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
    CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
    NLS_DATE_FORMAT="yyyy-mm-dd HH24:MI:SS"
    NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
    export EDITOR=vi
    export TMP TMPDIR ORACLE_TERM CLASSPATH NLS_DATE_FORMAT ORACLE_BASE ORACLE_HOME ORACLE_SID PATH LD_LIBRARY_PATH NLS_LANG EDITOR
    ```
    
## 上传解压安装包

> 操作节点：192.168.22.1

> 操作用户：root

1. 将安装包通过sftp或者客户端工具或者rz命令等方式上传至/tmp目录下

2. 解压压缩包

	```
	unzip linux.x64_11gR2_database_1of2.zip
    unzip linux.x64_11gR2_database_2of2.zip
	```

## 修改权限

> 操作节点：192.168.22.1

> 操作用户：root

```
chmod -R 777 /tmp/database
chown -R oracle.oinstall /tmp/database
```

## 创建app目录

> 操作节点：192.168.22.1

> 操作用户：oracle
    
```
mkdir -p /home/oracle/app/oracle
```
    
## 准备安装文件

> 操作节点：192.168.22.1

> 操作用户：oracle

1. 在/home/oracle/目录下新建目录etc，并执行以下命令

    ```
    mkdir /home/oracle/etc
    cp /tmp/database/response/* /home/oracle/etc/
    ```
2. 创建oraInst.loc文件，进入到/etc/目录下并新建oraInst.loc文件，并新增以下内容
    
    ```
    nventory_loc=/home/oracle/app/oraInventory
    inst_group=oinstall
    ```
3. 修改oraInst.loc文件权限
        
    ```
    chown oracle:oinstall /etc/oraInst.loc
    chmod 664 /etc/oraInst.loc
    ```

## 修改系统限制文件
    
> 操作节点：192.168.22.1

> 操作用户：root

1. 备份limits.conf

    ```
    cp /etc/security/limits.conf /etc/security/limits.conf.bak
    ```

2. 编辑/etc/security/limits.conf，在文件末尾加上以下内容(如果已经存在则跳过此步骤)

    ```
    oracle soft nofile 1024
    oracle hard nofile 65536
    grid soft nproc 2047
    grid hard nproc 16384
    grid soft nofile 1024
    grid hard nofile 65536
    ```
    
## 设置系统资源限制

> 操作节点：192.168.22.1

> 操作用户：root

1. 备份login
    
    ```
    cp /etc/pam.d/login /etc/pam.d/login.bak
    ```
2. 编辑/etc/pam.d/login，并在文件末尾加上以下内容(如果已经存在则跳过此步骤)
    
    ```
    session required pam_limits.so
    ```
## 设置系统环境变量

> 操作节点：192.168.22.1

> 操作用户：root

1. 备份profile
    
    ```
    cp /etc/profile /etc/profile.bak
    ```
    
2. 编辑/etc/profile，在文件末尾加上以下内容(如果已经存在则跳过此步骤)
    
    ```
    if [ $USER = "oracle" ]; then
       if [ $SHELL = "/bin/ksh" ]; then
           ulimit -p 16384
           ulimit -n 65536
        else
           ulimit -u 16384 -n 65536
       fi
    fi
    ```
    
3. 使环境变量生效
    
    ```
    source /etc/profile
    ```
    
## 设置系统参数文件

> 操作节点：192.168.22.1

> 操作用户：root

1. 备份sysctl.conf
    
    ```
    cp /etc/sysctl.conf /etc/sysctl.conf.bak
    ```

2. 编辑/etc/sysctl.conf，并在文件末尾加上以下内容(如果已经存在则跳过此步骤)

    ```
    fs.aio-max-nr = 1048576
    fs.file-max = 6815744
    kernel.shmall = 2097152
    kernel.shmmax = 1054472192
    kernel.shmmni = 4096
    kernel.sem = 250 32000 100 128
    net.ipv4.ip_local_port_range = 9000 65500
    net.core.rmem_default = 262144
    net.core.rmem_max = 4194304
    net.core.wmem_default = 262144
    net.core.wmem_max = 1048586
    net.ipv4.tcp_wmem = 262144 262144 262144
    net.ipv4.tcp_rmem = 4194304 4194304 4194304
    ```
    然后执行sysctl -p命令使其生效

## 配置oracle安装文件

> 操作节点：192.168.22.1

> 操作用户：oracle

1. 进入到/home/oracle/etc目录下，编辑db_install.rsp文件

    ```
    oracle.install.responseFileVersion=/oracle/install/rspfmt_dbinstall_response_schema_v11_2_0
    oracle.install.option=INSTALL_DB_SWONLY
    ORACLE_HOSTNAME=risen
    UNIX_GROUP_NAME=oinstall
    INVENTORY_LOCATION=/home/oracle/app/oraInventory
    SELECTED_LANGUAGES=en,zh_CN
    ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/db_1
    ORACLE_BASE=/home/oracle/app/oracle_base
    oracle.install.db.InstallEdition=EE
    oracle.install.db.isCustomInstall=true
    oracle.install.db.DBA_GROUP=dba
    oracle.install.db.OPER_GROUP=oinstall
    oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
    oracle.install.db.config.starterdb.globalDBName=orcl
    oracle.install.db.config.starterdb.SID=orcl
    oracle.install.db.config.starterdb.characterSet=ZHS16GBK
    oracle.install.db.config.starterdb.memoryOption=true
    oracle.install.db.config.starterdb.memoryLimit=512
    oracle.install.db.config.starterdb.installExampleSchemas=false
    oracle.install.db.config.starterdb.enableSecuritySettings=true
    oracle.install.db.config.starterdb.password.ALL=123456
    DECLINE_SECURITY_UPDATES=true
    ```

## 静默安装
> 操作节点：192.168.22.1

> 操作用户：oracle

1. 进入到/tmp/database目录下执行以下命令

    ```
    ./runInstaller -silent -ignorePrereq -responseFile /home/oracle/etc/db_install.rsp
    ```
    ![runInstaller](http://tva1.sinaimg.cn/large/007X8olVly1g71goscsmcj30ko053gll.jpg)
    需要等一会。若果没有问题，则继续下一步，如果有问题，查看相应的日志去解决，出现**Successfully.Setup.Software.**信息则说明安装成功。

## 检查
> 操作节点：192.168.22.1

> 操作用户：root

    ```
    sh /home/oracle/app/oracle/product/11.2.0/db_1/root.sh
    ```

## 安装监听
> 操作节点：192.168.22.1

> 操作用户：oracle

1. 完成之前步骤说明oracle的基本命令已经安装，并且在最开始我们已经将oracle的安装目录中的命令配置到了环境变量中，现在只需要使环境变量生效执行命令

    ```
    source /home/oracle/.bash_profile
    ```
2. 开始安装监听

    ```
    netca /silent /responsefile /home/oracle/etc/netca.rsp
    ```
    ![netcat](http://tva1.sinaimg.cn/large/007X8olVly1g71grwnng7j30fa05775e.jpg)
    (此处可能会报错命令解析错误，需要手动写入此命令，不然直接复制可能带有其它隐藏特殊字符)通过查看端口命令查看1521端口是否启动。
    
## 修改数据库配置并安装
> 操作节点：192.168.22.1

> 操作用户：oracle

1. 进入到/home/oracle/etc目录下，编辑dbca.rsp文件

    ```
    #数据库名.主机名
    GDBNAME = "orcl.risen"
    #设置实例名
    SID = "orcl"
    TEMPLATENAME = "General_Purpose.dbc"
    #设置sys账户密码
    SYSPASSWORD = "123456"
    #设置system账户密码
    SYSTEMPASSWORD = "123456"
    DATAFILEDESTINATION = /home/oracle/app/oracle/oradata
    RECOVERYAREADESTINATION= /home/oracle/app/oracle/oradata_back
    CHARACTERSET = "ZHS16GBK"
    TOTALMEMORY = "512"
    ```
    
    执行以下命令

    ```
    export DISPLAY=0.0
    dbca -silent -responsefile /home/oracle/etc/dbca.rsp
    ```
    (不要复制最好手动写，不然可能夹杂其它特殊字符)

    ![安装过程](http://tva1.sinaimg.cn/large/007X8olVly1g71gujeht4j30fs08074z.jpg)

## 查看监听状态
> 操作节点：192.168.22.1

> 操作用户：oracle

1.  查看监听状态

    ```
    lsnrctl status
    ```
    ![查看监听状态](http://tva1.sinaimg.cn/large/007X8olVly1g71gw9cfcej30ff07oq4u.jpg)

2. 配置监听文件

    ```
    cp /home/oracle/app/oracle/product/11.2.0/db_1/network/admin/samples/tnsnames.ora /home/oracle/app/oracle/product/11.2.0/db_1/network/admin/
    
    cp  /home/oracle/app/oracle/product/11.2.0/db_1/network/admin/samples/listener.ora /home/oracle/app/oracle/product/11.2.0/db_1/network/admin/
    ```

3. 修改监听名称解析文件**tnsnames.ora**，进入到/home/oracle/app/oracle/product/11.2.0/db_1/network/admin/目录下进行编辑**tnsnames.ora**在文件末尾新增

    ```
    orcl=
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.22.1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = orcl)
        )
      )
    ```

4. 修改监听文件listener.ora

    ```
    LISTENER =
     (ADDRESS_LIST=
            (ADDRESS=(PROTOCOL=tcp)(HOST=192.168.22.1)(PORT=1521))
            (ADDRESS=(PROTOCOL=ipc)(KEY=PNPKEY)))
    
    SID_LIST_LISTENER=
       (SID_LIST=
            (SID_DESC=
              (GLOBAL_DBNAME=orcl)
              (SID_NAME=orcl)
              (ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/db_1)
             (PRESPAWN_MAX=20)
              (PRESPAWN_LIST=
               (PRESPAWN_DESC=(PROTOCOL=tcp)(POOL_SIZE=2)(TIMEOUT=1))
             )
            )
           )
    ```

5. 重启监听

    ```
    lsnrctl stop
    lsnrctl start
    ```
    ![修改监听后的状态](http://tva1.sinaimg.cn/large/007X8olVly1g71gzmlwyij30ff077q4m.jpg)

# 启动oracle

> 操作节点：192.168.22.1

> 操作用户：oracle

1. 执行以sysdba的角色登录命令
    ```
    sqlplus / as sysdba
    ```
    ![sysdba_login](http://tva1.sinaimg.cn/large/007X8olVly1g71h19f9xgj30fd04k0tj.jpg)

2. 执行启动命令

    ```
    startup
    ```
    ![启动](http://tva1.sinaimg.cn/large/007X8olVly1g71h2an3t0j30g407caa5.jpg)
    
    说明启动成功，到此所有安装完成。可以新建一个用户然后通过Windows客户端去访问一下。

# 问题总结
- 问题一： 报错缺少DISPLAY变量
    
    > 解决一：在当前窗口执行

    ```
    export DISPLAY=0.0
    ```

- 问题二：lsnrctl: error while loading shared libraries: /u01/app/oracle/product/11.2.0/db_1/lib/libclntsh.so.11.1: cannot restore segment prot after reloc: Permission denied

    > 在root用户下执行以下命令
    
    ```
    su - root
    setenforce 0
    ```

- 问题三：错误 ORA-01102: cannot mount database in EXCLUSIVE mode
    > 参考链接 https://blog.csdn.net/lzwgood/article/details/26368323

- **最后附上linux安装oracle11g的脚本，具体安装过程通过查看日志可以解决百分之95以上的问题(自己写的，不足之处欢迎指正)**

```
#!/bin/bash +x
#author:ranzechen
#date:2019/05/06

oracle_config_path=./oracle.config

#判断oracle.config是否存在，不存在则创建
function config_file(){
	if [ ! -f $oracle_config_path ];then
		touch $oracle_config_path
		if [ $? == 0 ];then
			echo "=====>创建oracle配置信息成功<======"
			echo "##以下是oracle的默认配置信息，可修改" >> $oracle_config_path
			echo "#oracle的基础安装目录" >> $oracle_config_path
			echo "ORACLE_BASE=/home/oracle/app/oracle" >> $oracle_config_path
			echo "#oracle的安装目录" >> $oracle_config_path
			echo "ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/db_1" >> $oracle_config_path
			echo "#oracle的实例名" >> $oracle_config_path
			echo "ORACLE_SID=orcl" >> $oracle_config_path
			echo "#创建/etc/oraInst.loc文件" >> $oracle_config_path
			echo "nventory_loc=/home/oracle/app/oracle/oraInventory" >> $oracle_config_path
			echo "##配置oracle安装文件db_install.rsp" >> $oracle_config_path
			echo "#配置oracle启动密码" >> $oracle_config_path
			echo "oracle.install.db.config.starterdb.password.ALL=123456" >> $oracle_config_path
			echo "#配置sys密码" >> $oracle_config_path
			echo "SYSPASSWORD=123456" >> $oracle_config_path
			echo "#配置system密码" >> $oracle_config_path
			echo "SYSTEMPASSWORD=123456" >> $oracle_config_path
			echo "#配置oracle数据目录" >> $oracle_config_path
			echo "DATAFILEDESTINATION = /home/oracle/app/oracle/oradata" >> $oracle_config_path
			read -p "======>是否修改配置oracle的默认配置信息[yes/no]:" flag
                	if [ $flag == "yes" ];then
                        	vi $oracle_config_path
                        	exit
                	fi
		fi
	else
		read -p "======>是否修改配置oracle的默认配置信息[yes/no]:" flag
		if [ $flag == "yes" ];then
			vi $oracle_config_path
			exit
		fi
	fi
}

#配置oralce用户.bash_profile
function set_bash_profile(){
	echo "【配置oracle环境变量】"
	grep "ORACLE_HOME" /home/oracle/.bash_profile && grep "ORACLE_SID" /home/oracle/.bash_profile
	if [ $? != 0 ];then
		echo "umask 022" >>/home/oracle/.bash_profile 
		echo "stty erase ^H" >>/home/oracle/.bash_profile 
		echo "PATH=\$PATH:\$HOME/bin" >>/home/oracle/.bash_profile 
		echo "TMP=/tmp" >>/home/oracle/.bash_profile 
		echo "TMPDIR=\$TMP" >>/home/oracle/.bash_profile 
		echo `cat oracle.config | grep ORACLE_BASE=` >>/home/oracle/.bash_profile 
		echo `cat oracle.config | grep ORACLE_HOME=` >>/home/oracle/.bash_profile 
		echo `cat oracle.config | grep ORACLE_SID=` >>/home/oracle/.bash_profile 
		echo "ORACLE_TERM=xterm" >>/home/oracle/.bash_profile 
		echo "PATH=\$PATH:\$HOME/bin:\$ORACLE_HOME/bin" >>/home/oracle/.bash_profile 
		echo "LD_LIBRARY_PATH=\$ORACLE_HOME/lib:/lib:/usr/lib" >>/home/oracle/.bash_profile 
		echo "CLASSPATH=\$ORACLE_HOME/JRE:\$ORACLE_HOME/jlib:\$ORACLE_HOME/rdbms/jlib" >>/home/oracle/.bash_profile 
		echo 'NLS_DATE_FORMAT="yyyy-mm-dd HH24:MI:SS"'>>/home/oracle/.bash_profile 
		echo "NLS_LANG=AMERICAN_AMERICA.ZHS16GBK" >>/home/oracle/.bash_profile 
		echo "export EDITOR=vi" >>/home/oracle/.bash_profile 
		echo "export TMP TMPDIR ORACLE_TERM CLASSPATH NLS_DATE_FORMAT ORACLE_BASE ORACLE_HOME ORACLE_SID PATH LD_LIBRARY_PATH NLS_LANG EDITOR" >>/home/oracle/.bash_profile 	
	else
		echo "======>oracle用户.bash_profile环境变量可能已经配置,无需再次配置<======"
	fi
}

#yum安装oracle依赖库
function install_lib_env(){
	yum -y install  binutils-* compat-libstdc++-* compat-libcap1-* \
			elfutils-libelf-* elfutils-libelf-devel-* gcc* \
			gcc-c++-* glibc* glibc-common-* glibc-devel-* \
			glibc-headers-* ksh-* libaio-* libaio-devel-* \
			libgcc-* libstdc++-* libstdc++-devel* make-* \
			sysstat-* unixODBC-* unixODBC-devel-* numactl-devel-* \
			pdksh-* kernel-headers* uzip readline readline-devel
	if [ $? == 0 ];then
		echo "======>已成功安装oracle所需要的依赖库<======"
	else
		echo "======>检测不到可用的yum源<======"
		exit
	fi
}

#检测oracle的基础环境，如果没有则安装
function check_base_env(){
	echo "【检查当前用户】"
	sleep 2
	whoami
	if [ `whoami` != "root" ];then
		echo "======>请切换到root用户<====="
		exit
	fi
	echo "【检查是否有jdk环境】" 
	sleep 2
	java -version
	if [ $? != 0 ];then
		echo "======>没有检测到jdk<======"
		exit 0
	fi
	echo "【检查是否已经上传oracle安装包到/tmp目录下】"
	sleep 2
	ls /tmp/*database*.zip
	if [ $? != 0 ];then
		echo "======>没有检测到oracle安装包<====="
		exit 0
	fi
	echo "【检查是否有可用的yum源】"
	sleep 2
	install_lib_env
	echo "【检查是否存在oracle的默认配置信息】"
	sleep 2
	config_file
}
#创建oracle用户以及用户组
#配置oracle用户的环境变量以及创建oracle安装目录
function config_base_env(){
	#创建oinstall组和dba组以及oracle用户
	groupadd oinstall && groupadd dba && useradd -g oinstall -G dba oracle && echo "oracle" | passwd --stdin oracle
	set_bash_profile
	mkdir -p /home/oracle/app/oracle
	chmod -R 777 /home/oracle/app/oracle
	chown -R oracle.oinstall /home/oracle
	echo "【正在解压oracle压缩包】"
	sleep 2
	unzip -q -o /tmp/`ls /tmp/ | grep 1of2` -d /tmp/		
	unzip -q -o /tmp/`ls /tmp/ | grep 2of2` -d /tmp/
	chmod 777 /tmp
	chmod -R 777 /tmp/database
	chown -R oracle.oinstall /tmp/database
	echo -e "`cat oracle.config | grep nventory_loc=` \ninst_group=oinstall" > /etc/oraInst.loc
	chown oracle:oinstall /etc/oraInst.loc
	chmod 664 /etc/oraInst.loc
	mkdir /home/oracle/etc
	cp /tmp/database/response/* /home/oracle/etc/
}

#配置oracle运行环境参数
function config_run_env(){
	echo "【检测oracle运行环境limits参数并配置】"
	sleep 2
	grep "oracle" /etc/security/limits.conf
	if [ $? != 0 ];then
		cp /etc/security/limits.conf /etc/security/limits.conf.bak
		echo "oracle soft nproc 2047" >>/etc/security/limits.conf
		echo "oracle hard nproc 16384" >>/etc/security/limits.conf
		echo "oracle soft nofile 1024" >>/etc/security/limits.conf
		echo "oracle hard nofile 65536" >>/etc/security/limits.conf
		echo "grid soft nproc 2047" >>/etc/security/limits.conf
		echo "grid hard nproc 16384" >>/etc/security/limits.conf
		echo "grid soft nofile 1024" >>/etc/security/limits.conf
		echo "grid hard nofile 65536" >>/etc/security/limits.conf
	else
		echo "======>检测到oracle运行环境limits参数可能已经配置,无需再次配置<======"
	fi
	echo "【检测oracle运行环境sysctl参数并配置】"
	sleep 2
	grep net /etc/sysctl.conf && grep kernel /etc/sysctl.conf
	if [ $? != 0 ];then
                cp /etc/sysctl.conf /etc/sysctl.conf.bak
        	echo "fs.aio-max-nr = 1048576" >> /etc/sysctl.conf
		echo "fs.file-max = 6815744" >> /etc/sysctl.conf
		echo "kernel.shmall = 2097152" >> /etc/sysctl.conf
		echo "kernel.shmmax = 1054472192" >> /etc/sysctl.conf
		echo "kernel.shmmni = 4096" >> /etc/sysctl.conf
		echo "kernel.sem = 250 32000 100 128" >> /etc/sysctl.conf
		echo "net.ipv4.ip_local_port_range = 9000 65500" >> /etc/sysctl.conf
		echo "net.core.rmem_default = 262144" >> /etc/sysctl.conf
		echo "net.core.rmem_max = 4194304" >> /etc/sysctl.conf
		echo "net.core.wmem_default = 262144" >> /etc/sysctl.conf
		echo "net.core.wmem_max = 1048586" >> /etc/sysctl.conf
		echo "net.ipv4.tcp_wmem = 262144 262144 262144" >> /etc/sysctl.conf
		echo "net.ipv4.tcp_rmem = 4194304 4194304 4194304" >> /etc/sysctl.conf
		sysctl -p
	else
                echo "======>检测到oracle运行环境sysctl参数可能已经配置,无需再次配置<======"
        fi
	echo "【检测oracle运行环境login参数并配置】"
	sleep 2
	grep "pam_limits.so" /etc/pam.d/login
	if [ $? != 0 ];then
		cp /etc/pam.d/login /etc/pam.d/login.bak
		echo "session required pam_limits.so" >> /etc/pam.d/login
	else
                echo "======>检测到oracle运行环境sysctl参数可能已经配置,无需再次配置<======"
	fi
	echo "【检测oracle运行环境profile参数】"
        sleep 2
	grep "oracle" /etc/profile | grep "grid"
	if [ $? != 0 ];then
                cp /etc/profile /etc/profile.bak
        	echo 'if [ $USER = "oracle" ]||[ $USER = "grid" ]; then' >>  /etc/profile
		echo -e '\tif [ $SHELL = "/bin/ksh" ]; then' >> /etc/profile
		echo -e '\t\tulimit -p 16384' >> /etc/profile
		echo -e '\t\tulimit -n 65536' >> /etc/profile
		echo -e '\telse' >> /etc/profile
		echo -e '\t\tulimit -u 16384 -n 65536' >> /etc/profile
		echo -e '\tfi' >> /etc/profile
		echo 'fi' >> /etc/profile
		source /etc/profile
	else
                echo "======>检测到oracle运行环境profile参数可能已经配置,无需再次配置<======"
        fi
}

#oracle用户配置oracle安装文件install.rsp并运行安装命令
function config_install_rsp(){
	echo "【配置oracle安装文件】"
	sleep 2
	sed -i "s/oracle.install.option.*/oracle.install.option=INSTALL_DB_SWONLY/g" /home/oracle/etc/db_install.rsp
	sed -i "s/ORACLE_HOSTNAME=.*/ORACLE_HOSTNAME=`hostname`/g" /home/oracle/etc/db_install.rsp
	sed -i "s/UNIX_GROUP_NAME=.*/UNIX_GROUP_NAME=oinstall/g" /home/oracle/etc/db_install.rsp
	nventory_loc=`grep nventory_loc $oracle_config_path | awk -F '=' '{print$2}' | sed 's/\//\\\\\//g'`
	sed -i "s/INVENTORY_LOCATION=.*/INVENTORY_LOCATION=$nventory_loc/g" /home/oracle/etc/db_install.rsp
	sed -i "s/SELECTED_LANGUAGES=.*/SELECTED_LANGUAGES=en,zh_CN/g" /home/oracle/etc/db_install.rsp
	oracle_home=`grep ORACLE_HOME $oracle_config_path | awk -F '=' '{print$2}' | sed 's/\//\\\\\//g'`
	sed -i "s/ORACLE_HOME=.*/ORACLE_HOME=$oracle_home/g" /home/oracle/etc/db_install.rsp
	oracle_bath=`grep ORACLE_BASE $oracle_config_path | awk -F '=' '{print$2}' | sed 's/\//\\\\\//g'`
	sed -i "s/ORACLE_BASE=.*/ORACLE_BASE=$oracle_path/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.InstallEdition=.*/oracle.install.db.InstallEdition=EE/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.isCustomInstall=false.*/oracle.install.db.isCustomInstall=true/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.DBA_GROUP=.*/oracle.install.db.DBA_GROUP=dba/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.OPER_GROUP=.*/oracle.install.db.OPER_GROUP=oinstall/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.config.starterdb.type=.*/oracle.install.db.config.starterdb.type=GENERAL_PURPOSE/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.config.starterdb.globalDBName=.*/oracle.install.db.config.starterdb.globalDBName=`grep ORACLE_SID $oracle_config_path | awk -F '=' '{print$2}'`/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.config.starterdb.SID=.*/oracle.install.db.config.starterdb.SID=`grep ORACLE_SID $oracle_config_path | awk -F '=' '{print$2}'`/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.config.starterdb.characterSet=AL32UTF8.*/oracle.install.db.config.starterdb.characterSet=ZHS16GBK/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.config.starterdb.memoryLimit=.*/oracle.install.db.config.starterdb.memoryLimit=512/g" /home/oracle/etc/db_install.rsp
	sed -i "s/oracle.install.db.config.starterdb.password.ALL=.*/oracle.install.db.config.starterdb.password.ALL=`grep oracle.install.db.config.starterdb.password.ALL $oracle_config_path | awk -F '=' '{print$2}'`/g" /home/oracle/etc/db_install.rsp
	sed -i "s/DECLINE_SECURITY_UPDATES=.*/DECLINE_SECURITY_UPDATES=true/g" /home/oracle/etc/db_install.rsp

su - oracle << EOF
        cd /tmp/database

        ./runInstaller -silent -ignorePrereq -responseFile /home/oracle/etc/db_install.rsp

        install_log_path=`grep INVENTORY_LOCATION /home/oracle/etc/db_install.rsp | awk -F '=' '{print$2}'`/logs

        grep "Successfully Setup Software" $install_log_path/oraInstall*.out

        if [ $? == 0 ];then
                echo "======>oracle安装成功<======"
        else
                echo "======>oracle安装失败请切换到oracle用户进入到/tmp/database手动执行[./runInstaller -silent -ignorePrereq -responseFile /home/oracle/etc/db_install.rsp]并解决报错<======"
        fi
EOF
}
#安装oracle监听
function install_listener(){
	echo "【安装oracle监听】"
	sleep 2
su - oracle << EOF
	netca /silent /responsefile /home/oracle/etc/netca.rsp | tee -a ./listener.log
	grep successful ./listener.log
	if [ $? == 0 ];then
		echo "======>安装oracle监听成功<======"
	else
		echo "======>安装oralce监听失败请切换到oracle用户手动执行[netca /silent /responsefile /home/oracle/etc/netca.rsp]并解决报错"
	fi
EOF
}
#创建数据库
function config_database(){
	echo "【配置oracle数据库】"
	sleep 2
	value=`grep ORACLE_SID $oracle_config_path | awk -F '=' '{print$2}'`.`hostname`
	sed -i "s/GDBNAME =.*/GDBNAME = \"$value\"/g" /home/oracle/etc/dbca.rsp
	sid=`grep ORACLE_SID $oracle_config_path | awk -F '=' '{print$2}'`	
	sed -i "s/SID =.*/SID = \"$sid\"/g" /home/oracle/etc/dbca.rsp
	sys_passwd=`grep SYSPASSWORD $oracle_config_path | awk -F '=' '{print$2}'`
 	sed -i "s/.*SYSPASSWORD.*/SYSPASSWORD = \"$sys_passwd\"/g" /home/oracle/etc/dbca.rsp
	system_passwd=`grep SYSTEMPASSWORD $oracle_config_path | awk -F '=' '{print$2}'`	
 	sed -i "s/.*SYSTEMPASSWORD.*/SYSTEMPASSWORD = \"$system_passwd\"/g" /home/oracle/etc/dbca.rsp
	oracle_data=`grep DATAFILEDESTINATION $oracle_config_path | awk -F '=' '{print$2}' | sed 's/\//\\\\\//g'`
	sed -i "s/.*DATAFILEDESTINATION.*/DATAFILEDESTINATION=\"$oracle_data\"/g" /home/oracle/etc/dbca.rsp
	oracle_data_bak=`grep RECOVERYAREADESTINATION $oracle_config_path | awk -F '=' '{print$2}' | sed 's/\//\\\\\//g'`
	sed -i "s/.*RECOVERYAREADESTINATION.*/RECOVERYAREADESTINATION=\"$oracle_data_bak\"/g" /home/oracle/etc/dbca.rsp
	sed -i "s/.*CHARACTERSET.*/CHARACTERSET = \"ZHS16GBK\"/g" /home/oracle/etc/dbca.rsp
	sed -i "s/.*TOTALMEMORY.*/TOTALMEMORY=\"512\"/g" /home/oracle/etc/dbca.rsp
su - oracle << EOF
	
	export DISPLAY=0.0

	dbca -silent -responsefile /home/oracle/etc/dbca.rsp

EOF
}

#p配置监听文件
function config_listener(){
	echo "【开始配置监听】"
	sleep 2
	oracle_home=`grep ORACLE_HOME $oracle_config_path | awk -F '=' '{print$2}'`
	cp $oracle_home/network/admin/samples/tnsnames.ora $oracle_home/network/admin/
	cp $oracle_home/network/admin/samples/listener.ora $oracle_home/network/admin/
	sid=`grep ORACLE_SID $oracle_config_path | awk -F '=' '{print$2}'`
	ip=`ip addr | grep -w inet | sed -n '2p' | awk '{print$2}' | awk -F '/' '{print$1}'`
	echo "$sid=" >> $oracle_home/network/admin/tnsnames.ora
	echo "  (DESCRIPTION=" >> $oracle_home/network/admin/tnsnames.ora
	echo "     (ADDRESS_LIST=" >> $oracle_home/network/admin/tnsnames.ora
	echo "         (ADDRESS = (PROTOCOL = TCP)(HOST = $ip)(PORT = 1521))" >> $oracle_home/network/admin/tnsnames.ora
	echo "       )" >> $oracle_home/network/admin/tnsnames.ora
	echo "      (CONNECT_DATA =" >> $oracle_home/network/admin/tnsnames.ora
	echo "         (SERVICE_NAME = $sid)" >> $oracle_home/network/admin/tnsnames.ora
	echo "       )" >> $oracle_home/network/admin/tnsnames.ora
	echo "   )" >> $oracle_home/network/admin/tnsnames.ora

	echo "LISTENER = " >> $oracle_home/network/admin/listener.ora
	echo "	(ADDRESS_LIST= " >> $oracle_home/network/admin/listener.ora
	echo "        (ADDRESS=(PROTOCOL=tcp)(HOST=$ip)(PORT=1521)) " >> $oracle_home/network/admin/listener.ora
	echo "        (ADDRESS=(PROTOCOL=ipc)(KEY=PNPKEY)) " >> $oracle_home/network/admin/listener.ora
	echo "	) " >> $oracle_home/network/admin/listener.ora
	echo "SID_LIST_LISTENER= " >> $oracle_home/network/admin/listener.ora
	echo "   (SID_LIST= " >> $oracle_home/network/admin/listener.ora
	echo "        (SID_DESC= " >> $oracle_home/network/admin/listener.ora
	echo "          (GLOBAL_DBNAME=$sid) " >> $oracle_home/network/admin/listener.ora
	echo "          (SID_NAME=$sid) " >> $oracle_home/network/admin/listener.ora
	echo "          (ORACLE_HOME=$oracle_home) " >> $oracle_home/network/admin/listener.ora
	echo "          (PRESPAWN_MAX=20) " >> $oracle_home/network/admin/listener.ora
	echo "          (PRESPAWN_LIST= " >> $oracle_home/network/admin/listener.ora
	echo "			(PRESPAWN_DESC=(PROTOCOL=tcp)(POOL_SIZE=2)(TIMEOUT=1)) " >> $oracle_home/network/admin/listener.ora
	echo "          )" >> $oracle_home/network/admin/listener.ora
	echo "        )" >> $oracle_home/network/admin/listener.ora
	echo "    )" >> $oracle_home/network/admin/listener.ora
	chown oracle.oinstall $oracle_home/network/admin/{listener.ora,tnsnames.ora}
su - oracle << EOF

	lsnrctl stop

	lsnrctl start

	lsnrctl status | grep $ip
	
	if [ $? == 0 ];then
		echo "======>监听配置启动成功<====="
	else
		echo "======>监听配置启动失败，请排查问题<====="
	fi
EOF
}

#启动数据库
function startup_oracle(){
	echo "【启动数据库】"
	sleep 2
	sid=`grep ORACLE_SID $oracle_config_path | awk -F '=' '{print$2}'`
	ip=`ip addr | grep -w inet | sed -n '2p' | awk '{print$2}' | awk -F '/' '{print$1}'`
	pfile_path=`grep ORACLE_BASE $oracle_config_path | awk -F '=' '{print$2}'`"/admin/$sid/pfile"
	ora_filename=`ls $pfile_path | grep init.ora. | head -1`
	sed -i "s/local_listener=.*/local_listener=\"(ADDRESS=(PROTOCOL=tcp)(HOST=$ip)(PORT=1521))\"/g" $pfile_path/$ora_filename
su - oracle << EOF
	
	sqlplus / as sysdba
	
	startup pfile='$pfile_path/$ora_filename'
	
	exit

	ps -axu | grep smon
	if [ $? == 0 ];then
		echo "======>oracle启动成功<======"
	else
		echo "======>oracle启动失败<======"
	fi	
EOF
}

echo -e "======>使用必读<======"
echo -e "\033[1;41;33m 环境准备： \033[0m"
echo -e "    \033[31m 1.使用前请现将oracle安装包上传至/tmp下\033[0m"
echo -e "    \033[31m 2.配置好可用的yum源\033[0m"
echo -e "    \033[31m 3.建议安装好1.7版本的jdk\033[0m"
echo -e "\033[1;41;33m 安装顺序： \033[0m"
echo -e "    \033[31m 1.配置初始环境\033[0m"
echo -e "    \033[31m 2.安装oracle\033[0m"
echo -e "    \033[31m 3.安装监听\033[0m"
echo -e "    \033[31m 3.配置数据库\033[0m"
echo -e "    \033[31m 4.配置监听\033[0m"
echo -e "    \033[31m 4.启动数据库\033[0m"
read -p "======>如果尚未准备好环境请【Ctrl+c】结束脚本执行,[Enter]继续<======"
echo "================================================"
echo "检测初始环境[0] | 安装oracle[1] | 安装监听[2] | 配置数据库[3] | 配置监听[4] | 启动数据库[5]"
echo "================================================"
read -p "======>请选择你要安装的模块[0|1|2|3|4|5]:" num

case $num in
0)
	check_base_env && config_base_env && config_run_env
;;
1)
	config_install_rsp
;;
2)
	install_listener
;;
3)
	config_database
;;
4)
	config_listener
;;
5)	startup_oracle
;;
*)
	echo "你输入的数字不在选择服务提供范围内！"
;;
esac
```
