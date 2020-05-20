- 如何使用
  1. 导入包
    - hibernate-release-5.0.7.Final\\lib\\required\\*
    - (可用可不用)hibernate-release-5.0.7.Final\\lib\\optional\\c3p0\\*
    - log4j.jar
  2. 创建javabean
  3. 创建javabean和表之间的映射关系
    - 创建xx.hbm.xml配置文件(做javabean和表名的映射+做主键和属性的映射(包含主键自增方式)+其他属性和字段的映射关系)
    ```
      <?xml version="1.0" encoding="utf-8" ?>
      <!DOCTYPE hibernate-mapping PUBLIC
          "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
          "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

      <!--做表和javabean的映射关系-->
      <hibernate-mapping>
      <!--
      class标签：作用类和表的映射
          name：类的全限定名
          table：相对应的表名
      -->
      <class name="com.risen.bean.Customer" table="cst_customer">
          <!--
          id标签：做类中的某个属性和表的主键映射关系
          name：类的某个属性名
          column：表的主键字段名
          -->
          <id name="cust_id" column="cust_id">
              <!--
              generator：做主键自增长
                  class：增长方式(native：auto_increment)
              -->
              <generator class="native"></generator>
          </id>
          <!--
          property标签：其他属性和其他字段的映射关系
              name：类的属性
              column：对应的表字段名称
              length：字段长度(常用于自动创建表)
              not-null：非空(常用于自动创建表)
              unique：唯一(常用于自动创建表)
          -->
          <property name="cust_name" column="cust_name"></property>
          <property name="cust_source" column="cust_source"></property>
          <property name="cust_industry" column="cust_industry"></property>
          <property name="cust_level" column="cust_level"></property>
          <property name="cust_address" column="cust_address"></property>
          <property name="cust_phone" column="cust_phone"></property>
      </class>
      </hibernate-mapping>
    ```
  4. 编写连接数据库的核心配置文件xml**(必须在src下面，必须叫hibernate.cfg.xml)**
    ```
      <?xml version="1.0" encoding="utf-8" ?>
      <!DOCTYPE hibernate-configuration PUBLIC
          "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
          "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
      <!--配置hibernate连接数据库的信息-->
      <hibernate-configuration>
      <!--
          session-factory标签，类似生成session的工厂，session是connection
      -->
      <session-factory>
          <!--配置数据库的驱动-->
          <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
          <!--配置数据库的URL-->
          <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/web_demo?useSSL=false</property>
          <!--配置数据库的用户名-->
          <property name="hibernate.connection.username">root</property>
          <!--配置数据的密码-->
          <property name="hibernate.connection.password">123456</property>
          <!--数据库的方言，指的是不同数据相同功能使用的是不同的关键字,例如：limit-->
          <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
          <!--告诉hibernate使用c3p0-->
          <property name="hibernate.connection.provider_class">org.hibernate.c3p0.internal.C3P0ConnectionProvider</property>
          <!--在控制台显示hibernate自动生成的SQL语句-->
          <property name="hibernate.show_sql">true</property>
          <!--格式化控制台显示的SQL语句-->
          <property name="hibernate.format_sql">true</property>
          <!--加载映射文件-->
          <mapping resource="com/risen/Customer.hdb.xml"></mapping>
      </session-factory>
      </hibernate-configuration>
    ```
  5. 扩展
    - 如果需要hibernate根据映射关系自动生成数据库的表需要在`hibernate.cfg.xml`中配置
    ```
        <!--如果需要hibernate根据映射关系自动创建表需要配置以下项，默认不会创建
        create-drop：没有表创建表，有表删除掉创建表
        create：没有表创建表，有表了删除表在创建
        update：没有表创建表，有表更新表(常用)
        validate：默认值不创建
        -->
        <property name="hibernate.hbm2ddl.auto">validate</property>
    ```
