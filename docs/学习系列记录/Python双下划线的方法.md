## \_\_new__()

> 作用：在Python中存在于类里面的构造方法，\_\_init__()负责将类实例化，而在\_\_init__()启动之前，\_\_new__()决定是否要使用该\_\_init__()方法，因为\_\_new__()可以调用其他类的构造方法或者直接返回别的对象来作为本类的实例。

![new](http://tva1.sinaimg.cn/large/007X8olVly1g8hhc051q7j30i1095gm4.jpg)

## \_\_init__()

> 作用：作用：实例化该类时，自动执行\_\_init\_\_()方法定义的内容，但是\_\_init\_\_()一般不返回return

![init](https://s2.ax1x.com/2020/01/10/lhYxsS.png)

## \_\_getattr\_\_()

> 作用：当实例化该类后执行点调用对象时会执行\_\_getattr\_\_()方法

![getattr](https://s2.ax1x.com/2020/01/10/lhtnZ4.png)

## \_\_setattr\_\_()

> 作用：当实例化该类后执行点调用对象并存储值时，会执行\_\_setattr\_\_()方法

![setattr](https://s2.ax1x.com/2020/01/10/lhNm1P.png)

## \_\_delattr\_\_()

> 作用：当实例化该类后执行删除点调用的对象时，会执行\_\_delattr\_\_()


## \_\_call\_\_()

> 作用：能够让类的实例化对象，像函数一样被调用

![call](https://s2.ax1x.com/2020/01/10/lhNUXT.png)

## \_\_setitem\_\_()

> 作用：类似\_\_setattr\_\_()只是存放的对象是字典

## \_\_getitem\_\_()

> 作用：类似\_\_getattr\_\_()只是取的对象是字典

## \_\_delitem\_\_()

> 作用：类似\_\_delattr\_\_()只是删除的对象是字典

## \_\_enter\_\_()或\_\_exit\_\_()

> 作用：当使用with的时候回默认调用这两个方法
