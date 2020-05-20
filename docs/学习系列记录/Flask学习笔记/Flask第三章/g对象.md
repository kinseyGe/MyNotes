> g：global
1. g对象是专门用来保存用户的数据的。
2. g对象在一次请求中的所有的代码的地方，都是可以使用的。

```
from flask import Flask,g,request,render_template
from utils import login_log,login_ip

app = Flask(__name__)

# 只要是在这一次请求中就可以在这次请求的任何地方使用g对象
@app.route('/login/',methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    else:
        username = request.form.get('username')
        password = request.form.get('password')

　　　 　# 使用g对象
        g.username = username
        g.ip = password
        login_log()
        login_ip()
        return '恭喜登录成功！'

def login_log():
    print '当前登录用户是：%s' % g.username

def login_ip():
    print '当前登录用户的IP是：%s' % g.ip
if __name__ == '__main__':
    app.run
```
