> Maven：自动化构建工具

- 创建约定的目录结构
  - 工程名[根目录]
  - src目录[源码]
    - main目录[代码主程序]
      - java目录[存放java源文件]
      - resource目录[存放框架配置文件或者其他配置文件]
    - test目录[测试程序]
      - java目录[存放java源文件]
      - resource目录[存放框架配置文件或者其他配置文件]    
  - pom.xml[maven核心配置文件]


- Maven常用命令
  - mvn clean 清理
  - mvn compile 编译主程序
  - mvn test-compile 编译测试程序
  - mvn test 执行测试
  - mvn package 打包
  - mvn install 安装依赖到仓库(为自己的jar包生成仓库中的目录结构)


- pom文件
  - pom：Project Object Module 项目对象模型
    - 同相似的DOM Document Object Module 文档对象模型
    - pom.xml对于maven工程是核心配置文件重要性相当于web.xml对于web动态工程的配置
  - 坐标
    - maven中的坐标使用的就是下面三个向量在仓库中唯一定位一个maven工程
      - **g**roupid 公司/组织域名倒序+项目名 eg:com.risen.bigdata
      - **a**rtifactid 模块名 eg:spark
      - **v**ersion 项目版本
    - 由于上述的三个关键字是gav所以也叫gav工程


- 仓库
  - 本地仓库：当前电脑上部署的仓库目录
  - 远程仓库
    - 局域网范围内亦称为私服(nexus) ： 搭建在局域网环境中为局域网中的所有maven工程服务
    - 中央仓库：架设在internet为全世界所有maven工程服务
    - 中央仓库镜像：架设各大洲，为中央仓库分担压力


- 依赖
  - scope(范围)标签
    ```
    <scope>常用的有三个取值test,compile,provided</scope>
      compile范围依赖: 对主程序有效、对测试程序有效、参与打包
      test范围依赖: 对主程序无效、对测试程序有效、不参与打包
      provided范围依赖: 对主程序有效、对测试程序有效、不参与打包、不参与部署
    ```
  - 依赖原则
    - 路径最短者优先选择依赖
    - 相同路径以先声明者优先原则选择依赖


- 继承(统一版本管理)
  - 注意：配置继承后，执行mvn install命令时要先执行父工程，不然子工程无法依赖父工程install
  - 操作步骤
    - 创建一个maven工程作为父工程，注意打包方式为pom
    - 在子工程中声明对父工程的引用
    - 将子工程的坐标中与父工程坐标中重复引用的部分删除
    - 在父工程中统一Junit依赖
    - 在子工程中删除Junit依赖的版本号部分

  ```
  1. 父工程需要配置
  <packaging>pom</packaging>
  <dependencyMangement>
    <dependencies>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.9</version>
      </dependency>
    </dependencies>
  </dependencyMangement>

  2. 子工程配置
  <artifactId></artifactId>
  <parent>
    <groupId>父工程的groupID</groupId>
    <artifactId>父工程的artifactId</artifactId>
    <version>父工程的version</version>
    <relativePath>父工程的pom文件路径</relativePath>
  </parent>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
    </dependency>
  </dependencies>
  ```


- 聚合(解决继承逐级依赖问题)
  - 考虑到模块很多，依赖也很多，所以一键安装各个模块工程
  - 配置方式
    - 在一个"总的聚合工程"中配置各个参与聚合的模块
      ```
      在父工程中配置
      <modules>
        <module>
          指定各个子工程的相对路径
        </module>
      </modules>
      ```
  - 使用方式
    - 在聚合工程的pom.xml上点右键run as -> maven install
