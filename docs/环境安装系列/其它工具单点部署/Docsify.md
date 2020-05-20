> 使用背景：之前使用的是gitbook，并托管到GitHub上，但是页面风格不是很好看，而且选择插件的比较零散，用起来不是很灵活，导致页面风格和布局很死板。

> docsify：A magical documentation site generator.(神奇的文档站点生成器。) 官网地址： https://docsify.js.org/#/

> 下面简单的以本站为例来说一下docsify的使用过程

- 按照npm环境(不做过多介绍)

- 按照docsify
  ```
    npm i docsify-cli -g
  ```

- 创建项目(博客)目录
  ```
    我这里是D:\\MyNotes\\docs
  ```
- 进入到docs目录中初始化项目
  ```
    docsify init
  ```

- 在docs目录中启动项目即可
  ```
    docsify serve .
  ```

- 以上就是整个从安装到启动的过程，下面就是锦上添花的配置了，用一道华丽的分割线分一下

---

- 先简单的说一下init之后的目录结构
  - docs  项目目录
    - .nojekyll 系统文件不用关心
    - index.html 主页

- 然后说一下index.html到底有什么用，起始只要观察我的index.html中的注释就会明白了

```
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>网站主题</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <link rel="shortcut icon" href="配置网站icon" type="image/x-icon" />
    <meta name="description" content="Description">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <!-- 配置主题 -->
    <link rel="stylesheet" href="//unpkg.com/docsify/themes/vue.css">
  </head>
  <body>
    <div id="app"></div>
    <script>
      window.$docsify = {
        name: '首页网站标题', <!-- 定义网站主题 -->
        repo: '如果开启则此处是此项目的GitHub地址',  <!-- 是否开启GitHub挂件 -->
        coverpage: true, <!-- 开启渲染封面的功能 -->
        loadSidebar: true, <!-- 开始左侧目录结构 -->
        auto2top: false, <!-- 下一页后是否调到头部 -->
        search: {
          paths: 'auto',
          placeholder: '搜索一下...',
          noData: '没有搜索到任何内容...',
          depth: 6 // Headline depth, 1 - 6
        }
      }
    </script>
    <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
    <!-- emoji插件 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
    <!-- 复制代码到剪贴板插件 -->
    <script src="//cdn.jsdelivr.net/npm/docsify-copy-code"></script>
    <!-- 放大图片插件 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/zoom-image.min.js"></script>
    <!-- 分页插件 -->
    <script src="//unpkg.com/docsify-pagination/dist/docsify-pagination.min.js"></script>
    <!-- 搜索插件 -->
    <!-- <script src="//unpkg.com/docsify/lib/plugins/search.js"></script> -->
    <!-- 语言插件 包含java，c，Scala等多种语言-->
    <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-bash.min.js"></script>
    <!-- 目录折叠 -->
    <script src="//unpkg.com/docsify-sidebar-collapse/dist/docsify-sidebar-collapse.min.js"></script>
    <!-- gif插件 -->
    <link rel="stylesheet" href="//unpkg.com/docsify-gifcontrol/dist/docsify-gifcontrol.css">
    <script src="//unpkg.com/docsify-gifcontrol/dist/docsify-gifcontrol.js"></script>
  </body>
  </html>
```

!> 注意此处需要注意coverpage,loadSidebar,如果指定为true，那么需要在当前目录下分别创建_coverpage.md和_sidebar.md的文件

!> \_coverpage.md 配置的是覆盖主页的内容，比如可以指定一些介绍，logo等

!> \_sidebar.md 配置的是左侧目录树结构，其中的跳转我们可以使用相对路径进行跳转即可

*以上就是快速建站的主要流程了，具体细节还需结合官网查看。*

- 如果结合自己的域名发布呢？
  1. 需要在docs中创建一个叫CNAME的文件(没有.md),里面的内容即为你的完整域名`www.amcoder.cn`
  2. 登录GitHub并创建和你本地项目名称一样的仓库名称
  3. 在创建好的项目配置中GitHub Pages(Settings -> GitHub Pages)(Source选择**master branch/docs folder**，Custom domain填写你完整域名)
  4. 登录你购买域名的厂商官网进行填写域名解析，以百度域名为例，点击**解析**，然后**添加解析**，分别添加两条记录，一条主机记录为www，一条主机记录为@(作用是实现不用www直接解析)，记录类型全部选择**CNAME记录**，解析线路全部默认，记录值全部填写`GitHub用户名.github.io.`(不是账号是用户名),TTL全部默认即可。
