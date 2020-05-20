- 问题描述: Error starting daemon: error initializing graphdriver: /var/lib...VER>

![docker问题1](https://ftp.bmp.ovh/imgs/2019/09/2e2b890cba7b2f8f.png)

**解决方案**

```
尝试修改 /etc/sysconfig/docker-storage 为: DOCKER_STORAGE_OPTIONS="--storage-driver btrfs"
```
---

- 问题描述: error creating overlay mount to /var/lib/docker/overlay2

![docker问题2](https://ftp.bmp.ovh/imgs/2019/09/f0e15dcc6097d4b2.jpg)

**解决方案**

```
1. systemctl stop docker //停止docker服务

2. rm -rf /var/lib/docker //清理镜像

3. 修改存储类型

    vi /etc/sysconfig/docker-storage

    //把空的DOCKER_STORAGE_OPTIONS参数改为overlay:

    DOCKER_STORAGE_OPTIONS="--storage-driver overlay"

    //禁用selinux

    vi /etc/sysconfig/docker 去掉option的–selinux-enabled

4. systemctl start docker //启动docker服务
```
