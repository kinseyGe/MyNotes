- 问题描述

![kafka问题](https://ftp.bmp.ovh/imgs/2019/09/1a946fdfececb048.png)

**解决方案**

```
这是由于kafka重复消费导致的
```
---
- 问题描述

![副本问题](https://s2.ax1x.com/2019/11/27/QCYbo4.jpg)

**解决方案**

```
partitions在是在创建topic的时候默认创建的partitions节点的个数，只对新创建的topic生效，所有尽量在项目规划时候定一个合理的值。也可以通过命令行动态扩容。

./bin/kafka-topics.sh --zookeeper  localhost:2181 --alter --partitions 2 --topic  test
```
---
