- 使用
  1. 导入jquery.js文件
  2. 导入jquery-validate.js文件
  3. 导入message_zh.js文件
  4. 对表单进行校验
    - 在页面加载成功后获取表单对象
    ```
        表单对象.validate({
            rules:{}, //校验规则
            message:{} //自定义提示信息
          })
        rules写法：
          要校验的name属性:{
            校验器1:取值
            校验器2:取值
          }
        message写法：
          要校验的name属性:{
            校验器1:"自定义提示信息1"
            校验器2:"自定义提示信息2"
          }
    ```
  - 校验器
    - required true/false 必填校验
    - number true/false 数字校验
    - min 数字 最小值
    - max 数字 最大值
    - range 数值区间 [最小值,最大值]
    - minLength|maxLength|rangeLength 最小值|最大值|长度区间
    - email 邮箱校验(只校验了@符号)
    - equalTo jquery对象 重复性校验
  - 自定义校验器
    ```
        //参数：校验器名称，校验规则，提示信息
        $.validator.addMethod("校验器名称",function(value,ele,param){
            value：用户输入值,
            ele：当前输入的值所在的js对象,
            param：校验器的取值
            return true：符合校验器规则，反之不符合(默认)
          },"提示信息")
    ```
