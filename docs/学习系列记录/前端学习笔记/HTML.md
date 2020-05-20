- 超文本标记语言
  - 超文本： 普通文本只能显示文字，超文本除了显示文字之外还可以显示图片，视频，音频，文件特殊效果等
  - 标记：是HTML的组成元素,`<a> </a>` (亦称之为标签)
  - 语言：一门编程语言
- HTML(**H**yperText **M**arkup **L**anguage)

- 页面加载完毕事件
  ```
    window.onload = 函数名称;
    function 函数名称{

    }
    等价于
    window.onload = function{

    }
  ```

  ---
  - 下面用格式来说明标签
    ```
    <!DOCTYPE html>
    <html>
      	<head><!--保存了网页中的最重要的信息，而且这些信息打开网页看不见-->
          	<meta charset="utf-8"></meta><!--保存在重要信息，例如关键字和编码等等-->
          	<title>web-test</title><!--保存了本页的标题-->
      	</head>
      	<body><!--保存了网页中的内容，**不支持键盘换行**-->
      		<!--标题标签hn(1<=n<=6)
      			特点：自动换行，并且行与行之间有一定的间隔 -->
      		<h1>这是h1标题</h1>
      		<!--水平线标签hr
      			size属性：水平线的高度粗细
      			noshade属性：没有阴影就是纯色
      			px : html的像素单位
      		-->
      		<hr />
      		<!--字体标签
      			size属性：字体大小 只有 1<= size <= 7
      			color属性：字体颜色 两种表现形式(英文单词，#RGB-三原色)
      			face属性：表示字体(宋体...)
      		-->
      		<font size="5" color="red" face="楷体">这个是红色楷体的font标签</font>
      		<!--格式化标签
      			<b>标签： 加粗
      			<i>标签：倾斜
      		-->
      		<b>这是格式化标签中的加粗标签</b>
      		<i>这是格式化标签中的倾斜标签</i>
      		<b><i>这个是格式化标签中的既加粗又倾斜</i></b>
      		<!--段落标签和换行标签
      			<p>标签
      				特点：上下有一定的空白
      			<br/>标签
      		-->
      		<p>这是一个段落标签</p>
      		<p>这是一个段落标签</p>
      		演示换行标签<br />演示换行标签<br />
      		<!--图片标签
      			img
      				src属性
      				width属性
      		-->
      		<img src="img/TIM图片20191016111149.png" width="20%"/>
      		<!--列表标签
      			<ul></ul> unorder list 无序列表
      			<ol></ol> order list 有序列表
      			列表标签中都必须有列表项<li></li>
      			区别：ol每个列表项有一个序号，而ul每个列表项都是一样的
      			相同点：ol和ul都有一个共同的属性type(决定序号是数字还是英文等)
      		-->
      		<ol>
      			<li>这是有序列表第一项</li>
      			<li>这是有序列表第二项</li>
      		</ol>

      		<ul>
      			<li>这是无序列表第一项</li>
      			<li>这是无序列表第二项</li>
      		</ul>
      		<!--超链接标签
      			<a>内容</a>
      			href:被点击后跳转到哪个网页
      			target:指的是跳转后的目标位置(_self：在本页面打开 _blank：在新的页面打开)
      			framename:在指定的框架里打开
            -->
            <a href="http://www.baidu.com"></a>
      		<!--表格标签
    	  			<table>
    	  				<tr>
    	  					<th>表头，默认加粗，居中</th>
    	  				</tr>
    	  				<tr>
    	  					单元行，说白了有几行就代表有几个tr标签
    	  					<td>单元格标签，说白了有几个格子就有几个td标签</td>
    	  				</tr>
    	  			</table>
      			table属性
    	  			border：设置表格线
    	  			width：宽度
    	  			heigth：高度
    	  			align：整个表格在网页中的显示位置
    	  			bgcolor:设置背景颜色
    	  			cellpadding:单元格边缘与其内容之间的空白
    	  			cellspacing:单元格之间的空白
    	  		td属性
    	  			align：表示单元格中的内容居中
    	  		tr属性
    	  			align:表示改行单元格内容都居中
    	  		合并单元格，用到了td的两个属性
    		  		rowspan：跨行合并，具体的值指的是跨几行
    		  		colspan：跨列合并，具体的值指的是跨几列

            -->
            <table border="1px">
            	<tr>
            		<th>Header</th>
            		<th>合并2列</th>
            		<th>Header2</th>
            	</tr>
            	<tr>
            		<td>Data</td>
            		<td colspan="2">Data2</td>
            		<td>Data2</td>
            	</tr>
            </table>
            <!--表单标签
            	<form>
            		子标签：
        			<input>
        				type:
        					text：文本输入域，显示的文本内容(一般20个字符左右) 默认
        					password：密码输入框，显示的是星号代替文本
        					radio：单选框
        					checkbox:复选框
        					reset：重置按钮
        					button：普通按钮
        					submit：提交按钮
        						1.
        					(了解)image：图片按钮
        					hidder:隐藏域，数据会提交，但是页面上看不见
        					file:文件上传组件
        					1. 以上字标签都有一个共同的属性name属性
        					2. name作用
        						2.1一个格式给标签起名字
        						2.2另一个是给单选框和复选框划分组
        						2.3只有有name属性的空间上的值才可以提交，没有name属性不会被提交
        					3. 以上标签都有一个共同的属性value,给按钮的文字赋值
        					4. value作用
        						4.1当标签是单选框或者复选框，那么每一个选项必须有value值，否则提交时的值都是on
        						4.2如果option标签没有属性值默认就是内容
        			<textarea>文本域</textarea>
        				rows:设置行数
        				cols：设置列数
        			<select multiple="true">下拉框</select>
        			各种属性标签的默认值
        				在input标签 中type={text,password}是，如果设置了value，即为默认值
        				在input标签 中type=radio时，如果设置了checked 即为默认值
        				在select标签中，如果设置了selected 即为默认值
        			form的自身属性
        				aciton：表示你要把数据提交到哪个服务器，写的是URL地址
        				method：表示你要提交数据的方式get或post
            	</form>
            -->
            <form>
            	用户名：<input type="text" name="username"/><br />
            	密码：<input type="password" name="password"/><br />
            	您的性别：
            		男<input type="radio" name="sex" value="1" checked />
            		女<input type="radio" name="sex" value="0" /><br />
            	你的爱好：
            		抽烟<input type="checkbox" name="hobby" value="smoking"/>
            		喝酒<input type="checkbox" name="hobby" value="drinking"/>
            		烫头<input type="checkbox" name="hobby" value="tangtou"/><br />
            	您的头像：<input type="file" name="file" /><br />
            	您的简介：
            		<textarea name="desc" cols="20"></textarea><br />
            	您的住址：
            		<select name="address">
            			<option>河北省</option>
            			<option>浙江省</option>
            		</select><br />
            	<input type="submit" value="注册" /><br />

            	<input type="button" value="普通按钮"/><br />
            	<input type="reset" value="重置按钮" /><br />
            	<input type="image" value="图片按钮" src="img/TIM图片20191016111149.png" width="60px" height="30px"/>
            	<input type="hidden" />
            </form>
            <!--div标签，块级元素，独立站一行的元素

            -->
            <div>hello，我是div</div>
            <!--span，行内标签，特点不换行
            -->
            <span>我是span标签</span>
      	</body>
    </html>

    ```
