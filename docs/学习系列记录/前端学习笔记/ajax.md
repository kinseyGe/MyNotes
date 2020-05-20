- 异步javascript和xml(**a**synchronous **j**avascript **a**nd **x**ml)
  - 即使用JavaScript语言与服务器进行异步交互，传输数据为xml
- 应用场景
  - 检测用户名是否正确
  - 搜索下拉展示
  - 动态加载商品
- 优缺点
  - 优点
    - ajax无需加载整个页面,所以性能搞
  - 缺点
    - 无形中向服务器发送的请求多了，导致服务器压力增大
    - 由于ajax是在浏览器中使用javascript完成，所以还需要处理浏览器兼容性问题
---
- jquery中使用ajax
  - 方式一
    - `$.get(url,params,function(obj){},type)` 发送get请求
    - `$.post(url,params,function(obj){},type)` 发送post请求
    - 格式
      - url 请求路径
      - params 请求参数(key=value或者json格式)
      - function 成功之后的回调函数，obj是服务器返回给浏览器的内容
      - type 返回数据格式(json,xml,string,html,text)
  - 方式二
    ```
    $.ajax({
        url:请求地址,
        type:"GET",//默认
        data:请求参数,
        success:function(obj){},//成功之后的回调
        dataType:返回数据格式,
        async:是否异步，默认为异步 true
        error:function(obj){}//错误之后的回调
      })

      $("#btn2").click(function () {
          $.ajax({
              type:"get",
              url:"${pageContext.request.contextPath}/demoAjaxHttp",
              data:{"username":"李四"},
              success:function (obj) {
                  alert(obj)
              },
              error:function () {
                  alert("出现错误")
              }
          })
      })
    ```
