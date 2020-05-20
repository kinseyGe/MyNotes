- cookie中的key叫session
 
- 设置值session['key'] = '' 取值session.get('key')

- session的机制
    - 当请求到来后，flask读取cookie中的session，并将该值进行解密和反序列化以字典的形式放入内存；当请求结束时，flask会读取内存中字典的值，在进行序列化加密在写入到用户的cookie中
    - session可配置项：
    ```
    #因为请求到来时session是在cookie中然后被解密在反序列化的，在结束请求时加密，在序列化的，所以既然有加密解密就需要用到SECRET_KEY这个全局宏，一般设置为24个字符(os.urandom(24))
    'SECRET_KEY':  秘钥
    # 设置cookie中的session名称
    'SESSION_COOKIE_NAME':'session',
    'SESSION_COOKIE_DOMAIN':None,
    'SESSION_COOKIE_PATH':None,
    'SESSION_COOKIE_HTTPONLY':True,
    'SESSION_COOKIE_SECURE':False,
    # 设置每次请求之后的session过期时间开始计时
    'SESSION_REFRESH_EACH_REQUEST':True,
    # 设置session的过期时间
    'PERMANENT_SESSION_LIFETIME':timedelta(days=31),
    ```
- flash 在session存储数据，读取时通过pop将数据移除，以此创造出只存一次，只取一次的效果

- 设置session销毁时使用pop移除或者对字典操作删除也可以，但是flask有封装好的模块flash('临时数据存储','key')，取数据时使用模块get_flash_messages()