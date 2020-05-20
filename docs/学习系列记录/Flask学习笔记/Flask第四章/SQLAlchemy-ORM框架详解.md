1. 首先需要在__init__文件中实例化，并且
  - 一定要在导入蓝图的上面
  - 必须导入models.py（一般用于创建表之类的操作）

```
  from flask_SQLAlchemy import SQLAlchemy
  from .models import *
  db = SQLAlchemy()
```

2. 在实例化Flask的地方引入app等价于读取app中的配置的数据库连接信息

```
  db.init_app(app)
```

![配置数据库连接信息](http://tva1.sinaimg.cn/large/007X8olVly1g8hf0dlc8kj30mm0icta1.jpg
)

3. 在app配置文件中配置SQLAlchemy相关的配置

```
#配置基础公共环境
class BaseConfig(): # 通过继承来获取基础公共配置环境没有的flask配置项
    config = DevConfig()
    # MySQL连接
    SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://%s:%s@%s:3306/%s?charset=utf8' % (
        config.MYSQL_USERNAME,
        config.MYSQL_PASSWD,
        config.MYSQL_HOST,
        config.MYSQL_DBNAME)
    SQLALCHEMY_POOL_SIZE = 10
    SQLALCHEMY_MAX_OVERFLOW = 5
```

### 插入通过脚本生成表操作

4. 创建models.py的文件(创建表)，并在models.py中导入db

```
from sqlalchemy import Column
from sqlalchemy import Integer,String
from 项目名称 import db
class User(db.Model):#继承Model
    __tablename__ = 'users'
    id = Column(Integer,primary_key=True)
    name = Column(String(32),index=True,nullable=True)
    depart_id = Column(Integer)
```

5. 创建脚本create_table.py

```
from 项目名称 import db,create_app
app = create_app()
app_ctx = app.app_context()
with app_ctx:#这里的with等价于__enter__,通过localstack放入local中
    db.create_all()#执行创建表的方法
                   #默认最后还会调用一个__exit__的方法
```

### 基于ORM在蓝图中对数据库进行操作

```
from 项目名称 import db #相当于连接数据库
from 项目名称 import models #相当于导入表
from flask import Blueprint
index_bp = Blueprint('index_bp',__name__)
@index_bp.route('/index')
def index():
    #使用SQLALchemy在数据库中插入一条数据
    db.session.add(models.Users(name='xxx',depart_id='xxx'))
    db.session.commit()
    db.session.remove()
    #使用SQLALchemy在数据库中查询数据
    query_res = db.session.query(models.Users).all()
    print(query_res)
    db.session.remove()
    return 'index'
```

### 根据数据库中的表反向生成model

1. 安装sqlacodegen模块

2. 执行命令

```
C:\Users\THINKPAD> sqlacodegen mysql+pymysql://root:123456@192.168.5.57:3306/test > models.py
```

![反向生成model](http://tva1.sinaimg.cn/large/007X8olVly1g8hf44xwq1j30hs0153ya.jpg
)

3. 查看models.py

![查看model](http://tva1.sinaimg.cn/large/007X8olVly1g8hf4v7l8bj30ht0fdwf9.jpg
)
