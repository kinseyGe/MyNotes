> 作用：form表单验证和生成HTML标签

### 使用方法
- 用户登录
  - LoginForm.py
  - login.html
- 用户注册
  - RegisterForm.py
  - register.html
- 从数据库获取数据

- 用户登录demo
  - LoginForm
  ```
    from  flask import Flask,render_template,request,redirect
    from  wtforms.fields import core
    from wtforms.fields import html5
    from wtforms.fields import simple
    from wtforms import Form
    from wtforms import validators
    from wtforms import widgets
    app = Flask(__name__,template_folder="templates")

    class Myvalidators(object):
        '''自定义验证规则'''
        def __init__(self,message):
            self.message = message
        def __call__(self, form, field):
            print(field.data,"用户输入的信息")
            if field.data == "haiyan":
                return None
            raise validators.ValidationError(self.message)

    class LoginForm(Form):
        '''Form'''
        name = simple.StringField(
            label="用户名",
            widget=widgets.TextInput(),
            validators=[
                Myvalidators(message="用户名必须是haiyan"),#也可以自定义正则
                validators.DataRequired(message="用户名不能为空"),
                validators.Length(max=8,min=3,message="用户名长度必须大于%(max)d且小于%(min)d")
            ],
            render_kw={"class":"form-control"}  #设置属性
        )

        pwd = simple.PasswordField(
            label="密码",
            validators=[
                validators.DataRequired(message="密码不能为空"),
                validators.Length(max=8,min=3,message="密码长度必须大于%(max)d且小于%(min)d"),
                validators.Regexp(regex="\d+",message="密码必须是数字"),
            ],
            widget=widgets.PasswordInput(),
            render_kw={"class":"form-control"}
        )

    @app.route('/login',methods=["GET","POST"])
    def login():
        if request.method =="GET":
            form = LoginForm()
            return render_template("login.html",form=form)
        else:
            form = LoginForm(formdata=request.form)
            if form.validate():
                print("用户提交的数据用过格式验证，值为：%s"%form.data)
                return "登录成功"
            else:
                print(form.errors,"错误信息")
            return render_template("login.html",form=form)
    if __name__ == '__main__':
        # app.__call__()
        app.run(debug=True)
  ```

- login.html

  ```
    <body>
      <form action="" method="post" novalidate>
        <p>{{ form.name.label }} {{ form.name }} {{ form.name.errors.0 }}</p>
        <p>{{ form.pwd.label }} {{ form.pwd }} {{ form.pwd.errors.0 }}</p>
        <input type="submit" value="提交">
        <!--用户名：<input type="text">-->
        <!--密码：<input type="password">-->
        <!--<input type="submit" value="提交">-->
      </form>
    </body>
  ```

- 用户注册demo
  - register.py
```
    from flask import Flask,render_template,redirect,request
    from wtforms import Form
    from wtforms.fields import core
    from wtforms.fields import html5
    from wtforms.fields import simple
    from wtforms import validators
    from wtforms import widgets

    app = Flask(__name__,template_folder="templates")
    app.debug = True

    =======================simple===========================
    class RegisterForm(Form):
      name = simple.StringField(
          label="用户名",
          validators=[
              validators.DataRequired()
          ],
          widget=widgets.TextInput(),
          render_kw={"class":"form-control"},
          default="haiyan"
      )
      pwd = simple.PasswordField(
          label="密码",
          validators=[
              validators.DataRequired(message="密码不能为空")
          ]
      )
      pwd_confim = simple.PasswordField(
          label="重复密码",
          validators=[
              validators.DataRequired(message='重复密码不能为空.'),
              validators.EqualTo('pwd',message="两次密码不一致")
          ],
          widget=widgets.PasswordInput(),
          render_kw={'class': 'form-control'}
      )

    　　========================html5============================
      email = html5.EmailField(  #注意这里用的是html5.EmailField
          label='邮箱',
          validators=[
              validators.DataRequired(message='邮箱不能为空.'),
              validators.Email(message='邮箱格式错误')
          ],
          widget=widgets.TextInput(input_type='email'),
          render_kw={'class': 'form-control'}
      )

    　　===================以下是用core来调用的=======================
      gender = core.RadioField(
          label="性别",
          choices=(
              (1,"男"),
              (1,"女"),
          ),
          coerce=int  #限制是int类型的
      )
      city = core.SelectField(
          label="城市",
          choices=(
              ("bj","北京"),
              ("sh","上海"),
          )
      )
      hobby = core.SelectMultipleField(
          label='爱好',
          choices=(
              (1, '篮球'),
              (2, '足球'),
          ),
          coerce=int
      )
      favor = core.SelectMultipleField(
          label="喜好",
          choices=(
              (1, '篮球'),
              (2, '足球'),
          ),
          widget = widgets.ListWidget(prefix_label=False),
          option_widget = widgets.CheckboxInput(),
          coerce = int,
          default = [1, 2]
      )

      def __init__(self,*args,**kwargs):  #这里的self是一个RegisterForm对象
          '''重写__init__方法'''
          super(RegisterForm,self).__init__(*args, **kwargs)  #继承父类的init方法
          self.favor.choices =((1, '篮球'), (2, '足球'), (3, '羽毛球'))  #吧RegisterForm这个类里面的favor重新赋值

      def validate_pwd_confim(self,field,):
          '''
          自定义pwd_config字段规则，例：与pwd字段是否一致
          :param field:
          :return:
          '''
          # 最开始初始化时，self.data中已经有所有的值
          if field.data != self.data['pwd']:
              # raise validators.ValidationError("密码不一致") # 继续后续验证
              raise validators.StopValidation("密码不一致")  # 不再继续后续验证

    @app.route('/register',methods=["GET","POST"])
    def register():
      if request.method=="GET":
          form = RegisterForm(data={'gender': 1})  #默认是1,
          return render_template("register.html",form=form)
      else:
          form = RegisterForm(formdata=request.form)
          if form.validate():  #判断是否验证成功
              print('用户提交数据通过格式验证，提交的值为：', form.data)  #所有的正确信息
          else:
              print(form.errors)  #所有的错误信息
          return render_template('register.html', form=form)

    if __name__ == '__main__':
      app.run()
```

- register.html
```
  <body>
  <h1>用户注册</h1>
  <form method="post" novalidate style="padding:0  50px">
      {% for item in form %}
      <p>{{item.label}}: {{item}} {{item.errors[0] }}</p>
      {% endfor %}
      <input type="submit" value="提交">
  </form>
  </body>
```

- 从数据库获取数据实时读取数据

```
  from wtfroms import core
  from flask import Flask,request
  from wtforms import simple
  app = Flask(__name__)
  class UserForm(Form):
      city = core.SelectField(
          lable = '城市',
          choices = (),
          coerce = int #将choices中的id类型转为int类型
      )
      name = simple.StringField(lable='姓名')
      #每次实例化都从数据库实时读取数据
      def __init__(self,**args,**kwargs):
          super(UserForm,self).__init__(**args,**kwargs)
          self.city.choices = helper.fetch_all('select id,name from user',[],type=None) #这里需要注意的是，fetch_all的返回结果必须是元组类型

  @app.route('/user',methods=['GET','POST'])
  def user():
      if request.method == 'GET'
          #如果需要传入默认值编辑信息时，还可以在UserForm()中添加data参数，eg：UserForm(data={'name':'alex','city':1}),那么页面上的姓名输入框就会有默认值alex,而city默认会显示1对应的值
          form = UserForm(data={'name':'alex','city':1})
          #如果添加信息时
          #form = UserForm()
          return render_template('user.html',form=form)
  if __name__ == '__main__':
      app.run()
```
