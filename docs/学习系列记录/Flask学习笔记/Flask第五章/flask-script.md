## flask-script

> 作用：定制命令

```
from flask_script import  Manager #引入Manger模块
from CodeCount import init_app

app = init_app()
manager = Manager(app)#实例化Manger类

if __name__ == '__main__':
    manager.run()
```

> 通过命令启动项目

```
python manage.py runserver
```

1. 定制命令-按照位置传参

```
@manager.command
def custom(arg):#定制，例如当创建表时等可以使用此命令
    print(arg)
```

> 使用方法：python manage.py 函数名称 参数

```
python manage.py custom 123
```
![定制命令结果](http://tva1.sinaimg.cn/large/007X8olVly1g8hf7dkatyj30ao01n3ya.jpg)

2. 定制的命令-按照关键字传参

```
@manager.option('-n','--name',dest='name') #为定制的命令，定制参数可选项,使用方法：python manage.py cmd -n test -u http://www.test.com
@manager.option('-u','--url',dest='url')
def cmd(name,url):
    print(name,url)
```
> 使用方法：python manage.py 函数名称 -n name参数 -u url参数

```
python manage.py cmd -n test -u http://www.test.com
```

![定制命令结果](http://tva1.sinaimg.cn/large/007X8olVly1g8hf94bpm3j30g801rdfn.jpg)

## flask-migrate

> 作用：数据库表迁移 依赖flask-script

```
# -*- coding: utf-8 -*-
__author__ = 'ranzechen'
__time__ = 2019 / 3 / 20
__function__ = ''

from CodeCount import init_app
from CodeCount import db
from flask_migrate import Migrate,MigrateCommand

app = init_app()

Migrate(app,db)
'''
数据库迁移命令
python manage.py db init
python manage.py db migrate
python manage.py db upgrade
'''
manager.add_command('db',MigrateCommand)

if __name__ == '__main__':
    manager.run()
```
