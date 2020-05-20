**POM(Project Object Model)是Maven工程的工作基础，以pom.xml的形式存在项目中。在我们的工程中有几个元素是必须的：**

- project root
- modelVersion - 应被设置为4.0.0
- groupId - 项目组id
- artifactId - 项目id
- version - 项目版本

> 假设你有一个项目com.mycompany.app:my-app:1.0项目结构如下：

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-module</artifactId>
  <version>1.0</version>
</project>
```

> 如果我们想要my-module继承上级工程的pom配置，则可将pom.xml改为如下：

```
<project>
  <parent>
    <groupId>com.mycompany.app</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-module</artifactId>
  <version>1.0</version>
</project>
```

> 此外，如果我们希望工程的groupId跟version保持与父工程一致，则可去掉 groupId和version：

```

<project>
  <parent>
    <groupId>com.mycompany.app</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  <artifactId>my-module</artifactId>
</project>
```

> 下面就以一个比较完整pom.xml文件为例说明一下：

```
<project>
  <parent>
    <!--被继承的父项目的全球唯一标识符-->
    <groupId>com.mycompany.app</groupId>
    <!--被继承的父项目的构件标识符-->
    <artifactId>my-app</artifactId>
    <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号-->
    <version>1.0</version>
  </parent>
  <!--声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。--> 
  <modelVersion>4.0.0</modelVersion>
  <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 如com.mycompany.app生成的相对路径为：/com/mycompany/app--> 
  <groupId>com.mycompany.app</groupId>
  <!--构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有两个不同的项目拥有同样的artifact ID和groupID；在某个特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven为项目产生的构件包括：JARs，源码，二进制发布和WARs等。-->  
  <artifactId>my-module</artifactId>   
  <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号-->
  <version>1.0</version>
  <!--项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型-->  
  <packaging>jar</packaging>
  <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号-->  
  <!--(1表示大版本号，0表示小版本号)SNAPSHOT：快照，表示该版本正在开发中/release：稳定版本/beta：公测版/alpha：内部测试版/GA：正式发布版-->
  <version>1.0-SNAPSHOT</version>  
  <!--项目的名称, Maven产生的文档用-->  
  <name>项目名称</name>  
  <!--项目主页的URL, Maven产生的文档用-->  
  <url>项目主页URL</url>  
  <!--项目的详细描述,Maven产生的文档用。当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，就可以包含HTML标签）， 不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的索引页文件，而不是调整这里的文档。-->  
  <description>Maven工程的描述信息</description>   
  <!--开发人员的列表信息-->
  <developers>
    <developer>
      <name>开发者1</name>
      <!--等其他填写的信息-->
    </developer>
    <developer>
      <name>开发者2</name>
      <!--等其他填写的信息-->
    </developer>
  </developers>
  <!--许可证信息-->
  <licenses></licenses>
  <!--组织信息-->
  <organization></organization>
  <!--依赖项信息，依赖到的jar包-->
  <dependencies>
    <!--第一个依赖jar-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
      <type></type>
      <!--依赖的范围，表示本依赖应用于项目中的哪些阶段如下：
            compile：默认值。表示该依赖在编译、测试、运行阶段都有效
            provided：在编译和测试时有效，在运行时不会被加入
            runtime：在测试和运行时有效
            test：在测试范围内有效
            system：在编译和测试时有效，与provided类似，不过要与本地系统相关联，可移植性差
            import：在dependenceManagement中使用，表示导入别的项目的依赖到本项目中-->
      <scope>test</scope>
      <!--设置依赖是否可选，取值为true或false，默认是false，如果是false，则子项目必然继承父项目的依赖（不可选）,若为true，则子项目可以自己选择是否需要父项目的依赖，若需要就手动引入，若不需要就不引入-->
      <optional>true</optional>
      <!--排除依赖列表，如果a依赖b，b依赖c，那么默认的a依赖c，但是我a就是不想依赖c，则可以在这里排除掉c-->
      <exclusions>
        <exclusion></exclusion>
      </exclusions>
    </dependency>
    <!--第二个依赖jar-->
    <dependency>
      <!--....-->      
    </dependency>
  </dependencies>
  <!--一般为了统一管理多个项目，让他们的依赖都具有相同的版本，在所有子模块中的依赖标签都不指定明确的版本号，maven会自动向其父级查找，直到找到一个父模块拥有dependencyManagement标签，指定了所有依赖的版本号。这就保证所有模块的依赖版本都来自于同一个父模块的dependencyManagement指定。-->
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId></groupId>
        <artifactId></artifactId>
        <version></version>
        </dependency>
    </dependencies>
  </dependencyManagement>
  <!--插件对构建项目的支持-->
  <build>
    <!--插件-->
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-source-plugin</artifactId>
        <version>3.6</version>
        <!--表示该插件在什么时候执行-->
        <executions>
          <execution>
          <!--表示在打包阶段之后执行本插件-->
          <phase>package</phase>
          <!--执行方式，一般是与java的启动参数类似，例如：run等-->
          <goals>
            <goal>jar-no-fork</goal>
          </goals>
          </execution>
        </executions>
       </plugin>
    </plugins>
  </build>
  <!--多模块共同管理，一起编译-->
  <modules>
    <module>A</module>
    <module>B</module>
    <module>C</module>
  </modules>
  <!--属性，可以指定变量,如下所示-->
    <properties>
        <!--变量名称自己定义，获取变量值使用：${变量名称} -->
        <junit.version>4.10</junit.version>
    </properties>
</project>
```
