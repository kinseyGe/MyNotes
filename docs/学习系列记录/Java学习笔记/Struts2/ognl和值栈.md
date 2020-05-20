- 使用ognl步骤
  - 使用前提
    - ognl表达式不能再struts2中单独使用，必须要嵌套进struts2内置标签中使用
  1. 引入struts2标签
    ```
      <%@ taglib prefix="s" uri="/struts-tags" %>
    ```
  2. 使用struts2内置标签
    - 可以调用对象的方法(了解即可)`<s:property value="ognl表达式"/>`
    - 可以调用类的静态属性(了解即可)`<s:property value="@java.lang.Math@PI"/>`
    - 可以调用类的静态方法(了解即可)``
    - 可以获取值栈中的数据(**掌握**)


- 值栈
  - struts2提供的一个接口：valuestack
  - 创建过程
    > 当浏览器访问action的时候，会被前端控制器给拦截住(StrutsPrepareAndExecuteFilter)在前端控制器中，自动创建ValueStack对象(特点：访问一次，创建一次)

    > 当值栈对象被创建出来之后，会将当前访问的action对象整个放在值栈中，还会将request，session，servlet，servletContext的底层用来封装数据的map集合也放在值栈中(放的是引用地址)

    > 当整个action执行完毕后，action会销毁，值栈也跟着销毁，下一次在访问又是一个新的action对象和新的值栈对象

    > 所以值栈的生命周期是伴随着action一生的

  - valuestack内部结构
    - 重点说明context区和root区(用于存储数据)
    - context区(**不常用**)
      - 底层是继承了map结构
        - Map("request","request的map集合")
        - Map("session","session的map集合")
        - Map("application","servletcontext的map集合")
        - Map("attr","request,session和servletcontext的map集合")
        - Map("valuestack.....","valuestack的地址引用")
    - root区(**重点**)
      - 底层是继承了ArrayList结构
        - object
        - action对象

---
- 向valuestack的root区存数据
  - 成员属性的属性方式(**重中之重**)
    1. 定义成员属性
    2. 实现get方法
    3. 在页面使用ognl表达式(`<s:property value="username"/>`)，value值添加**#**说明从context区获取数据，没有#说明是在root区获取的数据

  - valuestack的api方式
    1. 获取root区
      ```
          CompoundRoot root = ActionContext.getContext().getValueStack().getRoot();
      ```

- ognl中的三个特殊符号
  - \# 作用
    1. 获取context区域中的数据
    2. 可以手动构建一个集合(struts2提供了很多页面标签)
  - % 作用
    1. 强制将标签内容转换为ognl表达式
  - $ 作用
    1. 获取值栈中的数据
