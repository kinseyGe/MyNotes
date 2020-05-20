- 查看正在运行的容器

```
docker ps
```
- 查看所有容器

```
docker ps -a
```

- 查看本地镜像

```
docker images
```

- 从镜像中运行/停止一个新实例

```
docker run/stop container
```

- 查看docker版本

```
docker --version
```

- 搜索镜像

```
docker search 镜像名称
```

- 拉取镜像

```
docker pull 远程仓库名称:镜像标签
```

- 运行某个容器并映射出端口

```
docker run -it --name 容器名称 -p IP地址:宿主机端口:容器端口 镜像仓库名称 /bin/bash
```

- 查看container详情

```
docker inspect [container ID]
```

- 删除某个container(其中container_id不需要输入完整，只要能保证唯一即可，运行中的Docker容器是无法删除的，必须先通过docker stop或者docker kill命令停止。)

```
docker rm [container ID]
docker rm `docker ps -a -q` 删除所有容器，-q表示只返回容器的ID
```

- 启动某个container

```
docker start container ID
```

- 查看某个container的运行日志

```
docker logs [container]
docker logs -f [container] 类似tailf
```

- 将container保存为一个image

```
docker commit [container ID] [image_name]
```

- 将image上传到远程仓库

```
docker push [image_name]
```

- 删除images

```
docker rmi [image ID]
```

- 删除untagged images，也就是那些id为<None>的image的话可以用

```
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
```

- 登录docker hub

```
docker login //这里的username不是docker id而是你的昵称
```

- 进入到某个容器

```
docker exec -it [container ID] /bin/bash
```

- 导出**镜像**到本地

```
docker save -o 本地镜像文件名称.tar 仓库名称
docker save 仓库名称 > 本地镜像文件名称.tar
```

- 导出**容器**到本地

```
docker export -o 本地镜像文件名称.tar 容器名称
docker export 容器ID/name > 本地镜像文件名称.tar
```

- 导入某个**容器**和**镜像**

```
 导入镜像
    docker load < 本地镜像.tar
    docker load --input 本地镜像.tar
 导入容器
    docker import 本地镜像.tar 镜像名称
```

- 重命名某个镜像

```
docker tag image_id 仓库名称:仓库标签
```

- 提交某个镜像到远程仓库

```
docker tag 本地的local-image:本地的tagname 远程的reponame:远程的tagname
//此时本地会出现一个远程仓库的image名字，相当于一个快捷方式连接到了本地，所以只需要提交远程的就行了
docker push 远程的repo:tag
```

- 如果需要依据容器创建镜像

```
需要先将容器导出,然后倒入容器并创建镜像

还可以实现对容器的重命名以及镜像的重命名
```
- 根据dockerfile构建容器

```
docker build -t repo .
```
