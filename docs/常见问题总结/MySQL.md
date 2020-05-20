- 问题描述: MySQL登录始终读取主机名

![MySQL登录始终读取主机名](https://s2.ax1x.com/2019/11/27/QCtJlq.jpg)

**解决方案**

```
在[mysqld]下设置skip-name-resolve即可
```
---
- 问题描述: ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

**解决方案**

```
select * from information_schema.innodb_trx

kill [trx_mysql_thread_id]
```
---
