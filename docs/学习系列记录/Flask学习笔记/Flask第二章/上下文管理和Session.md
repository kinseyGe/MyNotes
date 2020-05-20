# 上下文管理

- 偏函数

```
def sum(a1,a2):
    return a1 + a2

import functools
#偏函数为了帮助开发者传递函数的第一个参数
new_fun = functools.partial(sum,777)
res = new_fun(1)
print(res)
```

- 主动执行父类的构造方法

    - 子类需要继承父类，并且在需要执行父类的构造方法的方法中使用**super(子类名称,self).父类方法**进行调用
      
        > super 不是找父类，而是根据继承(__mro__)的顺序
    
    - 子类需要继承父类，并且在需要执行父类的构造方法的方法中使用父类名称.需要调用的函数(self)
    
        ```
        class Base(object):
            def fun(self):
                print('Base.fun')
        
        class Foo(Base):
            def fun(self):
                super(Foo,self).fun()#方式一
                #Base.fun(self) 方式二
                print('Foo.fun')
        
        if __name__ == '__main__':
            f = Foo()
            f.fun()
            #结果先打印了Base.fun后打印了Foo.fun
        ```
    
    - \_\_slots\_\_ 作用：slots中设置存放可以访问的对象
    
        ```
        class Base(object):
            __slots__ = ['name','age']
            def __init__(self):
                self.age  = 19
                self.name = 'zhangsan'
        
        if __name__ == '__main__':
            b = Base()
            print(b.name)
            print(b.age)
        ```
        
        ![slots](http://tva1.sinaimg.cn/large/007X8olVly1g6wqqqmrqej30l10dljs9.jpg)
        
        **错误结果**
        
        ![错误结果](http://tva1.sinaimg.cn/large/007X8olVly1g6wqroz0xmj30o90ecmxz.jpg)
        
        - Local类 作用：为线程或者协程开辟一个独立空间
        
        - LocalStack类 作用：在Local中将一个列表作为栈进行维护
    
# Session

> 通过导入flask_session来实现对以下配置的修改

```
from flask_session import Session
#('SESSION_TYPE', 'null')
#('SESSION_PERMANENT', True)
#('SESSION_USE_SIGNER', False)
#('SESSION_KEY_PREFIX', 'session:')
#('SESSION_REDIS', None)
#('SESSION_MEMCACHED', None)
#('SESSION_FILE_DIR', os.path.join(os.getcwd(), 'flask_session'))
#('SESSION_FILE_THRESHOLD', 500)
#('SESSION_FILE_MODE', 384)
#('SESSION_MONGODB', None)
#('SESSION_MONGODB_DB', 'flask_session')
#('SESSION_MONGODB_COLLECT', 'sessions')
#('SESSION_SQLALCHEMY', None)
#('SESSION_SQLALCHEMY_TABLE', 'sessions')
Session(app)
```
