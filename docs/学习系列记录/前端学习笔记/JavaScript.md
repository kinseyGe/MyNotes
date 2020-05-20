- JavaScript是一种解释型语言,不需要编译
- js分类
  - ECMAScript：js核心语法
  - BOM：浏览器对象
  - DOM：Document Object Model，操作文档中的内容和对象
- js必须嵌入到HTML中才能运行，两种引入方式
  - 内嵌式
    - 必须在`<script></script>`
  - 外联式
    - 写在另外一个文件，使用`<script></script>`标签引入刚刚引入的js文件
- 基本语法
  - 变量的命名
    - 不能使用关键字function命名
    - 严格区分大小写
    - 变量如果没有赋值，默认值undefiend
  - 格式 `var 变量名;`
- 基本类型
  - undefined
  - null
  - Boolean
  - number
  - String 必须用双引号和单引号引起来
- 基本操作
  - alert("") 弹出消息提示框
  - 函数
    - 不调用是不会运行的
    - 控制表单提交和不提交(见案例实现，用于控制不符合提交的表单)
      - 在绑定的函数中返回Boolean值，true表示提交，false表示不提交
      - 修改标签属性绑定函数的格式：事件名称="return 函数名称()"
    - 格式
    ```
      function 函数名(参数列表){

      }
    ```
- 案例实现
```
  引入js代码
    <script type="text/javascript" src="js/test.js"></script>

  HTML代码
    <form onsubmit="return tijiao()">
	     <span style="padding-right: 20px;">用户名</span><input type="text" name="username" id="username_id"/><br />
    </form>

  js代码
    function tijiao(){
    	username_value = document.getElementById("username_id").value
    	if(username_value == ""){
    		alert("用户名不能为空")
    		return false;
    	}else{
    		return true;
    	}
    }
```

- js中的正则
  - 第一种写法
    - var b = "需要判断规则的字符串".matches("/^正则表达式$/");
    - 如果b是true则满足规则，反之不满足
  - 第二种写法**推荐**
    - /^正则表达式$/.test("需要判断规则的字符串")


- 函数定义的拓展**不推荐**
  - 使用匿名函数`var fun1 = function(参数){函数体}`

---
- window对象是BOM中的内置对象，又称为全局对象
---
- 设置定时器
  - setInterval(code,毫秒值) 周期定时器，反复执行
    - **注意只执行了一次**，传入的函数如果带有括号传参那么需要引号引起来，除非不带参数可以直接写函数名，不用加引号
  - setTimeout(code,毫秒值) 一次性定时器，只会执行一次
- 取消定时器
  - clearInterval 取消周期定时器
  - clearTimeout 取消一次性定时器
---
- 修改标签的样式属性值(**注意获取到的属性的值是什么类型**)
  - obj.style.属性 ，获取指定"属性"的值
  - obj.style.属性 = 值 ，给指定"属性"设置内容
---
- 获取HTML某个标签的内容
  - 标签对象.innerHTML 获取到该标签的内容，即**开始标签和结束标签中间的内容**
---
- 相关事件
  - onsubmit 提交按钮被点击，提交按钮点击后，触发的表单提交事件
  - onblur 元素失去焦点，本来鼠标是选择该标签的，然后选中了别的标签的时候，就会触发失去焦点事件
  - onfocus 元素获得焦点，本来鼠标是选择别的标签，然后选中了该标签的时候，就会触发获取焦点事件
  - onclick 点击事件(常用于按钮)
  - onchange 值改变事件(常用于select下拉框)
  - onload 页面加载完事件 等价于 `body标签绑定onload`
  - onmouseover 鼠标移入事件
  - onmouseout 鼠标移出事件
---
- BOM 浏览器对象
  - window对象，一般用它的全局函数(onload，setTimeout，alert等)
    - alert 提示信息弹出框
    - confirm() 提示信息框(具有确定和取消按钮),当你点击确定是返回true，当你点击取消时返回false
    - prompt(),可以输入信息的提示框(具有确定和取消按钮)
  - navigator对象,导航
  - screen对象，可以获取屏幕的属性(长高款等)
  - history对象,浏览记录对象
  - location对象,当前浏览的网页地址
---
- 一个关键字`this`
  - 表示当前元素，谁调用的函数，在函数中的this就代表谁

---
- Array数组
  - 创建数组格式
    ```
      //元素类型可不一样
      new Array() 默认为0
      new Array(size) 指定数组长度
      new Array("element0","element1"...)
    ```
---
- dom对象(一般指的是document对象)
  - 元素获取
    - getElementById 根据id属性获取元素
    - getElementsByName 根据name属性获取元素
    - getElementsByClassName 根据class属性获取元素
    - getElementsByTagName 通过标签名称获取元素
  - 元素创建
    - document.createElement("标签名") 创建元素节点
    - document.createTextNode("文本内容") 创建文本节点
  - 常见属性
    - childNodes 获得所有子节点
    - nodeName 返回当前节点的名字
    - nodeType 返回该节点的类型(元素element，属性attribute，文本nodetext)
    - nodeValue 节点值，只有文本节点才有的，也就是节点内容
  - 添加子元素
    - setAttribute(name,value) 设置属性值
    - ele.appendChild(子元素) 向标签体末尾添加新的子节点
---
- 全局函数
  - parseInt() 把字符串解析成整数
  - parseFloat() 把字符串解析成小数
  - eval() 执行js脚本
  - encodeURI() 把字符串编码为URI
  - decodeURI() 把字符串解码为URI
