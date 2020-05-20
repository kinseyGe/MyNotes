- servlet入门(单实例多线程)
  - 方式一
    - 创建一个实现servlet接口的java类
    - 重写方法 service方法
    - 映射servlet
  - 方式二
    - 创建一个类继承HttpServlet
    - 重写方法 doXxx方法
    - 映射servlet
  - 两者的区别是，第一种方式是通过实现Servlet接口并重写了service方法，第二种方式是通过继承HttpServlet，而HttpServlet又实现了Servlet接口并重载了service方法
  ```
  <!--映射servlet-->
  <servlet>
      <servlet-name>Demo</servlet-name>
      <servlet-class>com.risen.web.demo.Demo</servlet-class>
  </servlet>
  ```
  - 绑定路径
  ```
  <!--绑定servlet路径-->
  <servlet-mapping>
      <servlet-name>Demo</servlet-name>
      <url-pattern>/firstdemo</url-pattern>
  </servlet-mapping>
  ```

- servlet体系结构
  - Servlet接口(生命周期：初始化，服务，销毁)
    - void init(ServletConfig config) 初始化方法
      > 第一次访问的时候，执行一次,执行者：服务器
    - void service(ServletRequest request,ServletResponse response) 服务方法
      > 每次访问的时候，访问次数等于执行次数，执行者：服务器
    - void destroy()销毁方法
      > 只有当服务被正常关闭或项目移除的时候，执行次数一次，执行者：服务器
  - GernericServlet抽象类
  - HttpServlet抽象类
