- 通过`ActionContext`的方式实现
  - 解耦合方式，只能用于接收参数 和 操作域中的数据
  ```
    //页面
      <h1>Servlet的API的访问方式一：解耦合的方式</h1>
      <form action="${ pageContext.request.contextPath }/requestDemo1.action" method="post">
          姓名：<input type="text" name="name"/><br/>
          年龄：<input type="text" name="age"><br/>
          <input type="submit" value="提交">
      </form>
    //action
        public class RequestDemo1Action extends ActionSupport{

        @Override
        public String execute() throws Exception {
          // 接收数据：
          /**
           * 在ActionContext中提供了一些方法:操作域对象的数据
           * 	* Map<String,Object> getParameters();
           * 	* Map<String,Object> getSession();
           *  * Map<String,Object> getApplication();
           */
          // 接收参数:
          ActionContext actionContext = ActionContext.getContext();
          Map<String,Object> map = actionContext.getParameters();
          for (String key : map.keySet()) {
            String[] arrs = (String[]) map.get(key);
            System.out.println(key+"    "+Arrays.toString(arrs));
          }
          // 向request域中存值：
          actionContext.put("reqName", "req如花");
          // 向session域中存值:
          actionContext.getSession().put("sessName", "sess凤姐");
          // 向application域中存值：
          actionContext.getApplication().put("appName","app石榴");
          return SUCCESS;
          }
        }
  ```

- 通过ServletActionContext对象的静态方法实现
  - 通过ServletActionContext对象静态方法获取
  ```
    //页面
      <h1>Servlet的API的访问方式三：通过ServletActionContext对象静态方法获取</h1>
      <form action="${ pageContext.request.contextPath }/requestDemo3.action" method="post">
          姓名：<input type="text" name="name"/><br/>
          年龄：<input type="text" name="age"><br/>
          <input type="submit" value="提交">
      </form>
    //action
        public class RequestDemo3Action extends ActionSupport{

        @Override
        public String execute() throws Exception {
          // 接收参数:
          /**
           * ServletActionContext在Struts2的API中的:
           * * HttpServletRequest getRequest();
           * * HttpServletResponse getResponse();
           * * ServletContext getServletContext();
           */
          HttpServletRequest request = ServletActionContext.getRequest();
          Map<String, String[]> parameterMap = request.getParameterMap();
          for (String key : parameterMap.keySet()) {
            String[] value = parameterMap.get(key);
            System.out.println(key+"   "+Arrays.toString(value));
          }
          // 向域中存值：
          request.setAttribute("reqName", "r郝宝强");
          request.getSession().setAttribute("sessName", "s郝喆");
          ServletActionContext.getServletContext().setAttribute("appName", "a郝蓉");
          return super.execute();
        }
      }
  ```

- 通过实现特定接口的方式实现(不推荐使用)
  - 通过实现`ServletRequestAware`,`ServletResponseAware`,`ServletContextAware`接口的方式
  ```
    //页面
      <h1>Servlet的API的访问方式二：实现特定接口的方式</h1>
      <form action="${ pageContext.request.contextPath }/requestDemo2.action" method="post">
          姓名：<input type="text" name="name"/><br/>
          年龄：<input type="text" name="age"><br/>
          <input type="submit" value="提交">
      </form>
    //action
        public class RequestDemo2Action extends ActionSupport implements ServletRequestAware,ServletContextAware{
        private HttpServletRequest request;
        private ServletContext application;

        @Override
        public String execute() throws Exception {
          // 接收数据：使用request对象。
          Map<String, String[]> parameterMap = request.getParameterMap();
          for (String key : parameterMap.keySet()) {
            String[] arrs = parameterMap.get(key);
            System.out.println(key+"   "+Arrays.toString(arrs));

          }
          // 向域中保存数据；
        //		向request域中保存数据
          request.setAttribute("reqName", "r郝三");
           // 向session域中保存数据
          request.getSession().setAttribute("sessName", "s李四");
          // 向application域中保存数据
          application.setAttribute("appName", "a王五");
          return NONE;
        }

        @Override
        public void setServletRequest(HttpServletRequest request) {
          this.request =  request;
        }

        @Override
        public void setServletContext(ServletContext application) {
          this.application = application;
        }
      }
  ```
