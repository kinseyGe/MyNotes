- 定义
  > 一套跨浏览器的js库，由于简化HTML与js之间的操作
- 使用jquery
  > 以外联式导入jquery对应的js文件即可，一般开发环境使用大的，而线上环境使用带min的
---
- jquery语法
  - 使用`jquery()` 或者 `$()`
  - 获取value值(id选择器)
    - $("$id值").val()
---
- js对象与jquery对象之间的转换
  - js对象转换为jquery对象
    - $(js对象)
  - jquery对象转换为js对象
    - 第一种方式 `jquery对象[index]`
    - 第二种方式 `jquery对象.get(index)`
---
- **页面加载成功事件**
  - js写法
    - window.onload = function(){}
  - jquery写法
    - $(document).ready(function(){})
    - `$(funciton(){})`
---
- 事件绑定
  - js方式
    1. 通过标签的事件进行绑定 `<xxx onclick="函数()">`
    2. 获取对象，`对象.事件属性 = function(){}`
  - jquery方式
    1. jquery对象.事件方法(function{})


- **jquery比较常用的事件**
  - `$()` 页面加载成功事件
  - `submit()` 表单提交事件
  - `click()` 鼠标单击事件
  - `focus()` 获取焦点事件
  - `blur()` 失去焦点事件
  - `change()` 内容发生改变事件
  - `keydown()` 键盘按下事件
  - `keypress()` 键盘按住事件
  - `keyup()` 键盘弹起事件
  - `dblclick` 鼠标双击事件
---
- jquery效果
  - 显示和隐藏
    - show([毫秒值]) 显示
    - hide([毫秒值]) 隐藏
    - toggle([毫秒值]) 切换隐藏和显示
  - 滑入和滑出
    - slidedown([毫秒值]) 滑入
    - slideup([毫秒值]) 滑出
    - slideToggle([毫秒值]) 切换滑入和滑出
  - 淡入和淡出
    - fadeIn([毫秒值]) 淡入
    - fadeOut([毫秒值]) 淡出
    - fadeToggle([毫秒值]) 切换淡入和淡出
---
- jquery选择器
  - 层次选择器
    - a b 选择器a选择的区域里面所有为选择器b的后代元素(嵌套)
    - a>b 选择器a选择的区域里面所有为选择器b的子代元素(嵌套)
    - a+b 选择器a选择的区域后面第一个为选择器b的弟弟的元素(同级)
    - a~b 选择器a选择的区域后面所有的为选择器b的弟弟的元素(同级)
  - 基本过滤选择器
    - `:first` 第一个
    ```
    $(function(){
        $("#button001").click(function(){
          $("#div001:first").css("background","red")
        })
    })
    ```
    - `:last` 最后一个
    - `even` 偶数
    - `odd` 奇数
    - `eq()` 等于某个index
    - `gt()` 大于某个index
    - `lt()` 小于某个index
- 内容过滤选择器
  - `:has(选择器)` 选取含有class为xx元素的div元素
- 可见性过滤选择器
  - `:visibel` 可见
  - `hidden` 不可见
- 属性选择器
  - [属性]
  - [属性=属性值]
---
- 对样式操作
  - jquery对象.css()
    - 获取
      - jquery对象.css("属性名")
    - 赋值
      - jquery对象.css("属性名","属性值")
  - 对多个css属性的操作
    ```
    jquery对象.css({
      "属性1":"属性值1",
      "属性2":"属性值2"
    })
    ```
  - 获取位置
    ```
    var $obj = jquery对象.offset()
    ```
- 对属性操作(一般attr使用起来有限制，如果不好使，那么改用**prop**)
  - jquery对象.attr()
    - 获取
      - jquery对象.attr("属性名")
    - 赋值
      - jquery对象.attr("属性名","属性值")
  - 对多个attr属性的操作
    ```
    jquery对象.attr({
      "属性1":"属性值1",
      "属性2":"属性值2"
    })
    ```
  - 删除属性
    - jquery.removeAttr("属性名")
  - 针对class属性的操作
    - jquery对象.removeClass("属性值")
    - jquery对象.addClass("属性值")
---
- jquery遍历
  ```
  方式1，一般使用
    jquery对象.each(function(index,ele){
      //this 遍历后的结果 js对象
      或者填写参数
      //ele 遍历后的结果 js对象
      //index 索引
      })
  方式2
    $.each(jquery对象,funciton(index,ele){
      //this 遍历后的结果 js对象
      或者填写参数
      //ele 遍历后的结果 js对象
      //index 索引
      })
  ```
---
- 对value html text的操作
  - value
    - 获取value值  `jquery对象.val()`
    - 设置值   `jquery对象.val("值")`
  - html
    - 获取value值  `jquery对象.html()`
    - 设置值   `jquery对象.html("值")`
  - text
    - 获取value值  `jquery对象.text()`
    - 设置值   `jquery对象.text("值")`
  - html和text设置值的区别
    - html会将标签直接解析到页面上，text会把标签当成普通文本展示在页面上
  - html和text获取值的区别
    - html会将标签体中存在的html标签获取出来，text不会把标签体中存在的html标签获取出来
---
- 文档操作
  - 内部插入，只插入到某个标签的内部
    - append : a.append(c) 把c插入到a内部的后面
    - prepend : a.prepend(c) 把c插入到a内部的前面
      **等价于**
    - appendTo : a.appendTo(c) 把a插入到c内部的后面
    - preppendTo : a.appendTo(c) 把a插入到c内部的前面
  - 外部插入，插入到同级标签前后
    - after : a.after(c) 把a插入到c外部的后面
    - before : a.before(c) 把a插入到c外部的前面
      **等价于**
    - insertAfter : a.insertAfter(c) 把c插入到a外部的后面
    - insertBefore : a.insertBefore(c) 把c插入到a外部的前面
  - 删除节点
    - empty 清空
    - remove 移除
---
- 表单对象属性选择器
  - enabled 可用的
  - disabled 不可用的
  - checked 可选的(针对单选和多选)
  - selected 可选的(针对下拉选)
---
- 事件切换
  - hover 模仿悬停事件(一个模仿悬停事件，鼠标移动到一个对象上面以及离开这个对象)
  - toggle 用于绑定两个或者多个时间处理器函数

- 同组标签取反(只选中同组标签中的一个标签)
  - siblings()

```
  $(function(){
		$("#div001").hover(function(){
			$("#div001").html("鼠标进入了")
		},function(){
			$("#div001").html("鼠标出去了")
		})

		$("#btn001").toggle(function(){
			alert(1)
		},function(){
			alert(2)
		})

    $(".yearConditionBtn").click(function() {
        $(this).css("background-color", "#3d88ee").siblings().css("background-color", "#FFFFFF")
    })
})
```
