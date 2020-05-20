1. 创建类时先执行type的__init__()方法 ,通过指定metaclass可以指定由哪个类创建

```
class MyType(type):
    def __init__(self,*args,**kwargs): #2.先执行
        super(MyType,self).__init__(*args,**kwargs)
    def __call__(cls,*args,**kwargs):
        obj = Foo.__new__(cls)
        cls.__init__(obj)
        return obj

class Foo(object,metalass=MyType): #1.创建类时
    def __init__(self):
        pass
    def __new__(cls,*args,**kwargs):
        return object.__new__(cls)
    def __func_(self):
        pass
```
2. 当一个类实例化时执行type的__call__()方法，__call__()方法的返回值就是实例化的对象

```
class MyType(type):
    def __init__(self,*args,**kwargs):
        super(MyType,self).__init__(*args,**kwargs)
    def __call__(cls,*args,**kwargs): #2.先执行
        obj = Foo.__new__(cls) #3.调用Foo的new方法 obj等于new的返回结果
        cls.__init__(obj) #4.调用Foo的构造方法
        return obj

class Foo(object,metalass=MyType):
    def __init__(self): #4.执行Foo的构造方法
        pass
    def __new__(cls,*args,**kwargs):#3.执行new方法
        return object.__new__(cls)
    def __func_(self):
        pass

obj = Foo() #1.实例化时
```
