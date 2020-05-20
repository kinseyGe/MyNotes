**以下我们在Linux操作系统上以sonarqube7.3为例来进行一下安装**
> 安装方式：windows基本等同Linux(个别步骤不同)

> SonarQube是一个用于代码质量管理的开源平台，用于管理源代码的质量，可以从七个维度检测代码质量,通过插件形式，可以支持包括java,C#,C/C++,PL/SQL,Cobol,JavaScrip,Groovy等等二十几种编程语言的代码质量管理与检测

# 环境准备
> maven

> [sonarqube7.3](https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.3.zip)，这里需要按照7.3的因为貌似只有7.3版本一下才支持MySQL5-8，而且是社区版

> 安装用户：非root用户(原因是，sonar安装包中自带es，安装过es的人都知道，es是不支持root安装的，所以统一使用非root用户安装maven和sonarqube)

# 开始安装

## 安装maven

1. 解压maven安装包
2. 修改maven解压目录下的conf目录中的setting.xml配置文件
```
  <!--配置本地仓库路径-->
  <localRepository>/opt/app/apache-maven-3.6.3/m2_repo</localRepository>
  <!--配置项目镜像源-->
  <mirrors>
      <mirror>
        <id>alimaven</id>
        <mirrorOf>central</mirrorOf>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      </mirror>
  </mirrors>

  <!--配置sonar的profile-->
    <profile>
      <id>sonar</id>
      <properties>
        <sonar.jdbc.url>jdbc:mysql://192.168.5.48:6500/sonar</sonar.jdbc.url>
        <sonar.jdbc.driver>com.mysql.jdbc.driver</sonar.jdbc.driver>
        <sonar.jdbc.username>root</sonar.jdbc.username>
        <sonar.jdbc.password>root</sonar.jdbc.password>
        <sonar.host.url>http://192.168.5.48:6599</sonar.host.url>
      </properties>
    </profile>
```
3. 将mvn加入到系统环境变量即可

## 开始安装sonarqube

1. 解压sonarqube安装包
2. 进入到解压目录下的conf目录中，新增以下配置
```
sonar.login=admin
sonar.password=admin
#我这里用的是MySQL，根据你的实际情况而定
sonar.jdbc.username=root
sonar.jdbc.password=root
sonar.jdbc.url=jdbc:mysql://192.168.5.48:6500/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
```

4. 启动进入到bin目录下，然后在进入到你相应的位数的操作系统中执行命令
```
./sonar.sh start
```
5. 打开浏览器输入ip:9000  账号：admin 密码：admin(这里我已经汉化，具体汉化看最后)
  - 首次登陆点击【分析新项目】(此处令牌可以是你名字说白了就是一个用户名)
  
![创建令牌](https://s1.ax1x.com/2020/04/24/JDlcLT.png)
  - 创建成功后选择项目的主要语言和开发技术(目前支持maven和gradle)
  - 复制右侧mvn命令,这个后面要用到
  ```
    mvn sonar:sonar \
      -Dsonar.host.url=http://192.168.5.48:6599 \
      -Dsonar.login=aadd6a805e8720de551fb7acb03aebdf6a442905
  ```

5. 使用git将项目clone到本地服务器上(如果此步骤已做过可跳过)

6. 进入到项目路径下(也就是带有pom.xml的同级目录下)，执行命令
```
  #安装忽略单元测试mvn插件
  mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true

  #执行分析命令(注意这里使我们从页面上复制的分析命令但是要指定sonar.java.binaries)
  mvn sonar:sonar -Dsonar.host.url=http://192.168.5.48:6599   -Dsonar.login=aadd6a805e8720de551fb7acb03aebdf6a442905 -Dsonar.java.binaries=./target/classes
```

7. 分析完成后，如果没有问题(如果有问题，只能根据构建过程自行百度了，因为我所遇到的，已经在文档中避免掉)

![mvn 构建成功](https://s1.ax1x.com/2020/04/24/JD1fnf.jpg)

8. 返回到浏览器刷新，显示出我们项目的分析结果

![分析结果](https://s1.ax1x.com/2020/04/24/JD3WG9.jpg)

9. 常见问题
  - 如果点击到bug中没有显示，任何数据，那么请重新执行第6步骤中的两条命令
  - 如果需要汉化，进入到【配置】 -> 【marktpalce】搜索chinese安装【Chinese pack】成功后重启服务即可
