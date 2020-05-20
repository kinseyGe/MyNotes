# el表达式
  - 概念
    - 它提供了在jsp中简化表达式的方法，让jsp代码更简单
  - 作用
    -替换<%=...%>
  - 语法
    - ${表达式} 遵循从小到大域的排查查找`${key}`
  - 功能
    - 只能获取**域**中的数据(格式：`${域对象.属性}` 或 `${域对象[属性值]}` 后者常用于处理属性值带有特殊符号的情况)
    ```
      四大域分别对应的取值对象为
        session  ： sessionScope
        application : applicationScope
        pageContext : pageScope
        request : requestScope

      四大域的大小为：
        pageContext < request < session < application
    ```
    - 执行运算
      - 使用empty判断对象是否为空或者null
    - el表达式中的内置对象
      - pageScope
      - requestScope
      - sessionScope
      - applicationScope
      - param
      - paramValues
      - header
      - headerValues
      - initParam
      - cookie
      - pageContext
  - 获取项目路径
    - `${pageContext.request.contextPath}`等价于`${pageContext.request.getContextPath()}`

# jstl标签
  - 概念
    - 是一个不断完善的开源代码的jsp标签库，由Apache维护
  - 使用步骤
    - 导入jar包
    - 在页面上导入标签库 `<%@taglib prefix="前缀" uri="需要的库"%>`
    ```
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```
    - jstl分类
      - core 核心
        - c:if
        ```
        //判断javabean是否为空
        原始写法
            <%
              if(products == null){
            %>
            <td colspan="4">暂无商品</td>
        jstl写法,products直接从setAttribute中拿的
            <c:if test="${products == null}">
              <td colspan="4">暂无商品</td>
            </c:if>
        ```
        - c:forEach
        ```
        //循环javabean
        原始写法
            <%
              }else{
                  for(Products p : products){
            %>
                    <tr>
                        <td><%= p.getId() %></td>
                        <td><%= p.getPname() %></td>
                        <td><%= p.getPprice()%></td>
                        <td><%= p.getPdesc() %></td>
                    </tr>
            <%
                  }
              }
            %>
        新的写法,products直接从setAttribute中拿的
            <c:if test="${products != null}">
              <c:forEach items="${products}" var="p">
                  <tr>
                      <td>${p.id}</td>
                      <td>${p.pname}</td>
                      <td>${p.pprice}</td>
                      <td>${p.pdesc}</td>
                  </tr>
              </c:forEach>
            </c:if>
        demo
            <c:forEach begin="1" end="10" step="1" var="i">
              ${i}
            </c:forEach>
        ```
      - fmt 国际化(格式化)
      - sql 和sql相关
      - xml 和xml相关
      - function 函数库
