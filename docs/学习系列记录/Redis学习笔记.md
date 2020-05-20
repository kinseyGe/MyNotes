> Redis入门指南(第二版)学习记录

- Redis(**R** emote **Di** ctionary **S** erver) 远程字典服务
- 支持存储的数据类型
  - 字符串类型 string
  - 散列类型 hash
  - 列表类型 list
  - 集合类型 set
  - 有序集合类型 zset

- 多数据库
  - redis每个数据库都是一个以0开始的递增名字命名，默认支持16个数据库，可以通过配置参数database来修改这一个数字，客户端连接后默认自动连接0号数据库
  - 切换1号数据库使用命令`select 1`
