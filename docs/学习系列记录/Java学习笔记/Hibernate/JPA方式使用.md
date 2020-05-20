- JPA(**J**ava **P**ersistence **A**PI-java的持久化api)
  - JPA是通过注解的方式来描述对象和表之间的映射关系

- JPA使用
  - 第一步在实体类中
    ```
      package com.risen.bean;

      import javax.persistence.*;

      /**
       * Author:
       * Date: 2019/12/19 14:43
       * Desc:
       */
      @Entity //声明持久化类
      @Table(name = "cst_customer") //映射类名和表名
      public class Customer {
          public String getCust_id() {
              return cust_id;
          }

          public void setCust_id(String cust_id) {
              this.cust_id = cust_id;
          }

          public String getCust_name() {
              return cust_name;
          }

          public void setCust_name(String cust_name) {
              this.cust_name = cust_name;
          }

          public String getCust_source() {
              return cust_source;
          }

          public void setCust_source(String cust_source) {
              this.cust_source = cust_source;
          }

          public String getCust_industry() {
              return cust_industry;
          }

          public void setCust_industry(String cust_industry) {
              this.cust_industry = cust_industry;
          }

          public String getCust_level() {
              return cust_level;
          }

          public void setCust_level(String cust_level) {
              this.cust_level = cust_level;
          }

          public String getCust_address() {
              return cust_address;
          }

          public void setCust_address(String cust_address) {
              this.cust_address = cust_address;
          }

          public String getCust_phone() {
              return cust_phone;
          }

          public void setCust_phone(String cust_phone) {
              this.cust_phone = cust_phone;
          }

          @Id //表示那个属性是oid属性
          @Column(name = "cust_id")  //oid属性和表的主键映射关系
          @GeneratedValue(strategy = GenerationType.IDENTITY)//声明主键的自增方式
          private String cust_id;// '客户编号(主键)'

          @Column(name = "cust_name") //其他属性和表中的字段映射关系(如果属性和字段值名称一样可以省略不写)
            private String cust_name ;// '客户名称(公司名称)'
          @Column(name = "cust_source")
            private String cust_source ;//'客户信息来源'
          @Column(name = "cust_industry")
            private String cust_industry ;// '客户所属行业'
          @Column(name = "cust_level")
            private String cust_level ;// '客户级别'
          @Column(name = "cust_address")
            private String cust_address ;// '客户联系地址'
          @Column(name = "cust_phone")
            private String cust_phone ;// '客户联系电话'
      }
    ```
  - 第二步需要在`src`目录下创建为`META-INF`的目录,并且在`META-INF`目录下需要创建`persistence.xml`
  ```
    <?xml version="1.0" encoding="utf-8" ?>
    <persistence xmlns="http://java.sun.com/xml/ns/persistence"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
               http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd" version="2.0">
      <!--至少得有一個持久化單元(至少得有一个数据库的连接信息，多个数据库连接信息复制多个persistence-unit标签即可)-->
      <persistence-unit name="MySQL" transaction-type="RESOURCE_LOCAL">
          <properties>
              <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver" />
              <property name="hibernate.connection.url" value="jdbc:mysql://localhost:3306/web_demo?verifyServerCertificate=false&amp;useSSL=false&amp;characterEncoding=UTF8" />
              <property name="hibernate.connection.username" value="root" />
              <property name="hibernate.connection.password" value="123456" />
              <property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect" />
              <property name="hibernate.connection.provider_class" value="org.hibernate.c3p0.internal.C3P0ConnectionProvider" />
              <property name="hibernate.show_sql" value="true" />
              <property name="hibernate.format_sql" value="true" />
              <property name="hibernate.hbm2ddl.auto" value="update" />
          </properties>
      </persistence-unit>
    </persistence>
  ```
 - 第三步写个demo测试一下
  ```
    public void test(){
      //加载数据库信息
      EntityManagerFactory mySQLFactory = Persistence.createEntityManagerFactory("MySQL");
      // 获取连接 等价于 session
      EntityManager entityManager = mySQLFactory.createEntityManager();
      // 获取事务
      EntityTransaction transaction = entityManager.getTransaction();
      //开始事务
      transaction.begin();

      Customer customer = new Customer();
      customer.setCust_name("周瑞成");
      //相当于session中的save方法
      entityManager.persist(customer);
      //提交事务
      transaction.commit();
      //关闭连接
      entityManager.close();
    }
  ```
