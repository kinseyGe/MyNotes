- 问题描述: 没有使用非root用户的其他用户启动

![image](https://ftp.bmp.ovh/imgs/2019/09/afbece1d37c3f330.jpg)

**解决方案**

```
切换到其他用户启动即可，记得修改elasticsearch目录权限以及日志目录和数据目录权限
```

---

- 问题描述: seccomp unavailable: requires kernel 3.5+ with CONFIG_SECCOMP and CONFIG_SECCOMP_FILTER
- [4]ERROR: bootstrap checks failed

![image](https://ftp.bmp.ovh/imgs/2019/09/fbd19f566d1d4e64.jpg)

**解决方案**

>修改elasticsearch.yml中配置在Memory下面添加下面两个属性:

```
bootstrap.memory_lock: false

bootstrap.system_call_filter: false
```

---

- 问题描述: [1]max file descriptors [4096] for elasticsearch process likely too low, increase to at least [65536]

**解决方案**

> 切换到root用户，编辑limits.conf配置文件， vim /etc/security/limits.conf添加以下内容：

```
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
```

---

- 问题描述: [2]max number of threads [1024] for user [bigdata] likely too low, increase to at least [2048]

**解决方案**

> 切换到root用户，进入limits.d目录下，修改90-nproc.conf 配置文件，vi /etc/security/limits.d/90-nproc.conf,找到如下内容：

\* soft nproc 1024 修改为 \* soft nproc 2048

---

- 问题描述: [3]max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]

**解决方案**

> 切换到root用户下，修改配置文件sysctl.conf，vi /etc/sysctl.conf添加下面配置

```
vm.max_map_count=655360
```
保存退出并执行命令

```
sysctl -p
```

---

- 问题描述: unable to load JNA native support library, native methods will be disabled

**解决方案**

> 备份之前的lib库，然后在GitHub下载jna-5.0.0.zip文件，然后将里面的dist目录下的jna.jar替换掉之前lib库中jna-4.4.0.jar

---

- 问题描述: max size virtual memory [4930928640] for user [es-xue] is too low, increase to [unlimited]

**解决方案**

> 切换到root用户，编辑limits.conf配置文件， vim /etc/security/limits.conf添加以下内容：

```
* hard memlock unlimited
* soft memlock unlimited
*  - as unlimited
```
