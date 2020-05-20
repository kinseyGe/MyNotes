- filter(过滤器,单实例多线程)
  - 是2.3之后servlet增加的一个新功能，运行在服务器端，先与之相关的servlet或者jsp页面之前运行
  - 作用：过滤请求和响应(一个路径可以有多个过滤器)
  - 应用场景：自动登录，统一编码，过滤一些特殊符号或者敏感词
  - 入门程序
    - 实现一个filter接口
      `implements Filter`
      - 注意别忘记放行请求和响应`filterChain.doFilter(servletRequest,servletResponse); `
    - 重写所有方法(filter的声明周期方法)
      ```
        重写 init 方法
        重写 doFilter 方法
        重写 destroy 方法
      ```
    - 注册filter
    - 绑定路径
    ```
      <filter>
          <filter-name>过滤器名字</filter-name>
          <filter-class>过滤器类路径</filter-class>
      </filter>

      <filter-mapping>
          <filter-name>过滤器名字</filter-name>
          <url-pattern>需要过滤的请求路径</url-pattern>
      </filter-mapping>
    ```
  - url-pattern配置
    - 完全匹配以"/"开始，例如:/demo1 /demo2
    - 目录匹配以"/"开始，以"*"结束，例如:/aa/* /*
    - 后缀名匹配以"*"开始，例如:*.jsp *.html
  - 过滤链
    - 多个filter组合在一起
    - 执行顺序
      > 多个filter的执行顺序是由web.xml中的filter-mapping的位置决定的，当一个filter收到请求的时候，调用chain.doFilter才可以访问下一个匹配的filter，若当前的filter是最后一个filter，调用chain.doFilter才能访问目标资源
  - filter-mapping中的子标签
    - servlet-name 指定标签过滤那个servlet
    - dispatcher 指定过滤哪种方式过来的请求(*可以指定多个*)
      - REQUEST 默认值，只过滤从浏览器发送过来的请求
      - FORWARD **只过滤请求转发过来的请求**
  - 全局的统一错误页面(*可以配置多个*)
    ```
      <error-page>
          <error-code>404</error-code>
          <location>/404.jsp</location>
      </error-page>
    ```
  - filterConfig
    - 获取filter的名称
    - 获取filter初始化参数
    - 获取filter所有初始化名称
    - 获取上下文
