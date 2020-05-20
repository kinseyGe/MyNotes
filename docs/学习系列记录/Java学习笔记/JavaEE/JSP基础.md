- jsp(**J**ava **S**erver **P**age)
  - 组成部分
    - html + java代码 + **jsp特有的内容**
  - 工作流程
    > 第一次访问服务器中index.jsp的时候，首先会加载服务器中的web.xml文件，通过反射机制找到JspServlet来进行处理，服务器会将index.jsp文件转为java文件，服务器会在把java文件编译成clas文件，服务器执行class文件，产生一个响应把结果传递给服务器，再由服务器响应给浏览器，由浏览器进行解析
  - jsp特有内容
    - jsp脚本
      - <%...%> java程序片段 **生成在service中**
      - <%=...%> 输出表达式 **生成在service中相当于调用了out.print(...),并且表达式不能以分号结尾**
      - <%!...%> 声明成员 **不会出现在service中**
  - jsp注释
    - <%-- 内容 --%>
  - jsp中的四个域对象
    - pageContext 当前页面
    - request 当前请求
    - session 当前会话
    - application 当前项目
  - jsp的一个动作标签
    - <jsp:动作标签 属性="值">
      - <jsp:forward> 在jsp页面中完成请求转发
      ```
      <jsp:forward page="/login"></jsp:forward>
      ```
      - <jsp:include> 动态包含，将被包含资源的运行结果包含进来
