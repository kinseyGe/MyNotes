- Configuration
  - 用于加载核心配置文件 hibernate.cfg.xml
  - 用于加载映射文件(**基本不用**)
    - 手动加载 addSource()


- SessionFactory
  - session工厂，内部维护的hibernate**连接池**，一般一个应用只需要创建一个对象


- Session 连接对象
  - 是hibernate持久化操作的核心api
  - get和session的区别
    - 第一个区别
      - session.get 是立即查询，发送SQL语句
      - session.load 是延迟查询，只有当用到调用返回对象的时候才会生成SQL去查询数据库
    - 第二个区别
      - get查询返回的是javabean对象
      - load查询返回的是javabean的代理类型
    - 第三个区别
      - get查询不到返回null
      - load查询不到返回报错信息
  - 从连接池中获取session
    - sessionFactory.openSession()
  - 从当前线程中获取绑定的session,在多层之间调用方法获取的都是同一个session
    - 特点
      - 默认是关闭的，需要配置开启,配置核心配置文件hibernate.cfg.xml中的`<property name="hibernate.current_session_context_class">thread</property>`
      - 会自动关闭连接
    - sessionFactory.getCurrentSession()
