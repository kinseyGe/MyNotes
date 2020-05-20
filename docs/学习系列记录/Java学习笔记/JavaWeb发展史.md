# 第一阶段 servlet阶段
- 上世纪90年代，随着Internet和浏览器的飞速发展，基于浏览器的B/S模式随之火爆发展起来。 最初，用户使用浏览器向WEB服务器发送的请求都是请求静态的资源，比如html、css等。 但是可以想象：根据用户请求的不同动态的处理并返回资源是理所当然必须的要求，java 为了应对上述需求，就必然推出一种技术来支持动态需求，因此**servlet**技术诞生
```
public void doGet(HttpServletRequest request,HttpServletResponse)
   throws IOException,ServletException
{
    response.setContentType("text/html;charset=gb2312");
    PrintWriter out = response.getWriter();
    out.println("<html>");
    out.println("<head><title>Hello World！</title></head>");
    out.println("<body>");
    out.println("<p>Hello World！</p>");
    out.println("</body></html>");
}
```
# 第二阶段 Jsp的出现
- servlet诞生后，sun公司很快发现servlet编程很繁琐
  1. servlet代码有大量冗余代码，out输出就得写上百遍；
  2. 开发servlet必须精通网页前端和美工，你得非常不直观的在Servlet中写前端代码，这使得实现各种页面效果和风格非常困难。所以，sun公司借鉴 微软的asp,正式推出了**jsp（servlet1.1）**
```
<html>
   <head><title>测试</title></head>
   <body>
      第二阶段<% String str = "test"; out.println(str); %>
    </body>
</html>
```

# 第三阶段 jsp+javabean+servlet
- 诞生背景：倡导了MVC思想的servlet版本servlet1.2出现，jsp出现后，也存在问题
  1. 前端开发人员需要看大量他看不懂的后端代码
  2. 同样，servlet开发人员也在复杂的前端代码中找到其能写servlet代码的地方所以，**MVC思想的JSP(V)+JavaBean(M)+Servlet(C)诞生了**

# 第四阶段之框架阶段

## 框架演变图

  ```
    SSH(Struts、Spring、Hibernate)
                ↓   后来升级为Struts2
    SSH(Struts2、Spring、Hibernate)
                ↓   到后来Struts2被spring mvc替换掉
    SSH(SprintMVC、Spring、Hibernate/ibatis)
                ↓   然后Hibernate配置维护需要的人力成本大而慢慢被ibatis代替，后来MyBatis出现了，从iBatis到MyBatis，MyBatis提供了更为强大的功能，同时并没有损失其易用性
    SSM(SpringMVC、Spring、Mybatis)
                ↓
    Springboot、Mybatis
  ```


## Struts框架

- 倡导了MVC思想的jsp+javabean+servlet出现，也存在问题
  1. jsp页面中嵌入了很多java代码，使得结构很乱；
  2. 对于大型项目，servlet过多，转向频繁，流程，配置等不易集中管理，因而出现了**struts**
- 2001年6月，struts1.0出现，struts针对jsp推出了一套struts标签，从而使得jsp中没有了Java代码，结构清晰，功能强大。针对servlet，它提供了Action类来代替了servlet，这个Action类具有servlet的功能，并且能够进行一些请求过滤和自动转码的功能。

## Spring框架阶段

> 原本已经开起来很完美了，但是又有一个问题，就是我们在Action调用DAO、Java bean等对象的时候都需要在自身代码中构建它们的对象来使用，这样增加了程序的耦合性，这与我们：“高内聚、松耦合”的思想不符合，那么怎么解决这个问题呢？因而出现了**Spring框架**。

  - Spring框架有两大功能：IOC（控制反转）和AOP（面向切面的编程），其中IOC就是说：当一个类中想要调用另外一个类的对象时，不需要再通过new 关键字来创建，而是由Spring框架来负责：创建、分配和管理，从而降低了程序中的耦合性。而AOP可以用来做一些日志的打印和输出，用于提示程序执行过程中的一些具体信息等。

## SpringMVC的出现
> 最后struts和Spring的整合，由于每一个bean都要在Spring中注册，每一个URL都要在struts配置文件中配置。当bean很多和URL对应的请求很多的时候，配置文件无疑会是很庞大的，这个就会使得配置起来很麻烦的费力。那么还有没有更好的办法使得能够结合Spring的功能和struts的功能，但是又可以使配置文件不会批量的增加？因而**SpringMVC**出现了

  - SpringMVC通过"基于注解"的方式代替了struts，并且通过Controller类来代替和实现了Action的功能。由于是基于注解的，所以很多的配置信息放在了Controller类中配置，从而降低了.xml文件的配置复杂度。
