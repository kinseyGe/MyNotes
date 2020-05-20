# cookie
  - 浏览器端会话技术
  - api
    - 创建cookie `new Cookie(String name,String value)`
    - 返回浏览器  `response.addCookie(Cookie c)`
    - 获取cookie `Cookie [] request.getCookies()`
      - 获取cookie的名称 `getName()`
      - 获取cookie的value `getValue()`
      - 设置cookie在浏览器端的存活时间 `setMaxAge(int 秒数=0?删除:秒数)`前提是路径相同
      - 设置cookie的路径 `setPath(String cookiepath)` **当访问的url包含此cookie的path的时候，就会携带这个cookie；反之不会**
    - 注意cookie不能夸浏览器，并且不支持中文

# session
  - api
    - session获取 `HttpSession session = request.getSession();`
    - session属性操作
      - session.xxxAttribute(); (set,get,remove)
    - 生命周期
      - 创建
        - java代码中暂时认为第一次调用request.getSession()
      - 销毁
        - 服务器非正常关闭
        - 超时
          - 默认时间 tomcat 30分钟
          - 手动设置 `session.setMaxInactiveInterval();`
          - 手动销毁 `session.invalidate();`
