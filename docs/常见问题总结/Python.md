- 问题描述: 在linux环境下部署Python项目时常常报错无法找到自己编写的模块

**解决方案**

```
export PYTHONPATH=项目路径
```
---

- 问题描述:Scrapy防封之settings文件设置

**解决方案**

- 设置动态USER-AGENT

  - 安装scrapy-fake-useragent模块

  - 在settings.py中添加配置

```
DOWNLOADER_MIDDLEWARES = {
   'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware':None,# 关闭默认方法
   'scrapy_fake_useragent.middleware.RandomUserAgentMiddleware':400,# 开启
}
```
- 问题描述: 设置download_delay

> 他的作用主要是设置下载的等待时间，下载等待时间长，不能满足段时间大规模抓取的要求，太短则大大增加了被ban的几率

```
DOWNLOAD_DELAY = 3
```

- 问题描述: 禁止cookies

```
COOKIES_ENABLES=False
```

---

- 问题描述: pyinstaller 打包报错

![pyinstaller打包报错](http://tva1.sinaimg.cn/large/007X8olVly1g8hfuhetqdj314006pjrq.jpg)

**解决方案**

> 重新编译Python3并安装，进入到Python解压目录执行命令

```
./configure --prefix=/usr/local/python3/ --enable-shared
make && make install
```
