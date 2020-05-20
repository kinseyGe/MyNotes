### setting.xml顶层标签如下

```
<span style="font-family:Microsoft YaHei;"><settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0  
            http://maven.apache.org/xsd/settings-1.0.0.xsd">  
    <localRepository/>  
    <interactiveMode/>  
    <usePluginRegistry/>  
    <offline/>  
    <pluginGroups/>  
    <servers/>  
    <mirrors/>  
    <proxies/>  
    <profiles/>  
    <activeProfiles/>  
</settings></span>  
```

**各个标签解释：**

- localRepository

> 建构系统本地仓库的路径，不设置的话默认是在{user.home}/.m2/repository/下，如果想要系统所有用户共用一个本地仓库，则可以在maven安装目录下的setting.xml中进行设置

- interactiveMode

> 指定Maven是否试图与用户交互来得到输入，默认是true 

- usePluginRegistry

> 如果设置为true，则在{user.home}/.m2下需要有一个plugin-registry.xml来对plugin的版本进行管理。默认是false

- offline

> 如果不想每次编译的时候都去查找远程中心仓库，就需要设置为true，但前提是本地仓库中已有需要的jar包，默认是false

- pluginGroups

> 该元素包含一系列的pluginGroup元素，每个pluginGroup又有一个groupId，当一个plugin被使用而在命令行中哦给没有指定groupId的时候，就会查询这个列表

- Servers

> maven除了一般的本地仓库和中央仓库之外，还有一种是远程仓库，一般部署在局域网中供Maven用户使用（成为私服），当maven需要下载构件的时候，它先从私服中请求，如果没有，再到外部的中央仓库中下载，同时下载的构件会在下载到私服中供以后使用，或者用户可以将将构件上传到私服中。私服还有一个好处就是存放组织内部自己生成的私有构件，这类构件不可能从外部的中央仓库获取，但是组织内部用户又需要共享使用，这个时候就需要私服了。

> 一般私服建立完毕之后不需要认证就可以访问，但是处于安全方面的考虑，需要提供认证信息才能访问这些私服，这时就需要使用servers元素（需要注意的是配置私服的信息是在pom文件中，但是认证信息则是在setting.xml中，这是因为pom文件往往是被提交到代码仓库中供所有成员访问的，而setting.xml是存放在本地的，这样是安全的）。

> 而maven是根据pom中的repositories和distributionMnagement元素来决定，然后运行maven clean deploy，这样maven就根据pom中的配置将自己的第三方构件部署在私服上供组织内其他用户使用（注意maven clean deploy和maven clean install的区别：deploy是将该构件部署在私服中，而install是将构件存入自己的本地仓库中）。

- morriors

> 镜像，供maven下载jar包

- proxies

> 当用户用代理登录下载时需要配置

### 提供几个好用的镜像地址

- 阿里云镜像地址

```
<mirror>
    <id>nexus-aliyun</id>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

- 华为镜像地址

```

<mirror>
    <id>huaweicloud</id>
    <name>mirror from maven huaweicloud</name>
    <url>https://mirror.huaweicloud.com/repository/maven/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

- ibiblio 镜像地址

```

<mirror>
    <id>ibiblio</id>
    <name>Mirror from Maven ibiblio</name>
    <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```


- maven官方的二号仓库

```
<mirror>
    <id>repo2</id>
    <name>Mirror from Maven Repo2</name>
    <url>http://repo2.maven.org/maven2/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

- JBoss的仓库

```
<mirror>
    <id>jboss-public-repository-group</id>
    <mirrorOf>central</mirrorOf>
    <name>JBoss Public Repository Group</name>
    <url>http://repository.jboss.org/nexus/content/groups/public</url>
</mirror>
```
