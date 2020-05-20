- 作用
  - **资源共享(servlet通信)**
  - 获取资源文件


- 生命周期
  - 创建：服务器启动的时候会为每个项目创建一个servletcontext上下文对象，servletcontext是项目的一个引用
  - 销毁：在服务器关闭或者项目移除的时候servletcontext销毁


- ServletContext上下文
  - 获取
    - getServletConfig.getServletContext()
    - getServletContext()
  - 常用方法
    - String getInitParameter(String name) 获取指定的项目初始化参数
    - Enumeration getInitParameterNames() 获取项目所有初始化参数
    - String getRealPath(String filePath) 获取一个资源在服务器上的路径
    - InputStream getResourceAsStream(String filePath) 以流的方式返回一个文件
    - String getMimeType(String 文件名) 获取一个文件的mime类型text/html image/gif

    ```
      <!--在web.xml中添加全局上下文参数-->
      <context-param>
          <param-name>db</param-name>
          <param-value>mysql</param-value>
      </context-param>
    ```
    ```
      ServletContext sc = getServletContext();
      System.out.println(sc.getInitParameter("db"));
      Enumeration<String> initParameterNames = sc.getInitParameterNames();
      while (initParameterNames.hasMoreElements()){
          String element = initParameterNames.nextElement();
          System.out.println(element + ":" + sc.getInitParameter(element));
      }
      String realPath = sc.getRealPath("/index.jsp");
      System.out.println(realPath);
      String mimeType = sc.getMimeType("/index.html");
      System.out.println(mimeType);

      输入结果：
      /**
        *mysql
        *db:mysql
        *D:\web\demo\out\artifacts\web_demo_war_exploded\index.jsp
        *text/html
        */
    ```
  - 资源共享
    - `setAttribute(String name,Object value)` 设置属性
    - `getAttribute(String name)` 获取指定属性
    - `removeAttribute(String name)` 删除指定属性
