- 防火墙相关(Centos)

  - 临时关闭防火墙命令(同理开启start)

    ```
    systemctl stop firewalld.service

    systemctl stop firewalld.service
    ```

  - 永久关闭防火墙命令

    ```
    systemctl disable firewalld
    ```

  - 查看防火墙状态

    ```
    firewall-cmd --state
    ```

  - 添加某个tcp端口到至端口白名单(同理添加udp)

    ```
    firewall-cmd --zone=public --add-port=22122/tcp --permanent
    firewall-cmd --zone=public --add-port=22122/udp --permanent
    ```

  - 使端口生效

    ```
    firewall-cmd --reload
    ```

  - 查看白名单添加的端口

    ```
    firewall-cmd --list-ports
    ```

- 配置免秘钥相关

  - 生成公钥

    ```
    ssh-keygen -t rsa 一路回车
    ```

  - 发送公钥命令

    ```
    ssh-copy-id -i ~/.ssh/id_rsa.pub 对端主机名
    ```
