![html](http://img.blog.csdn.net/20161028091119701)

![html](http://img.blog.csdn.net/20161102015958085)

## 什么是HTML？
- 全称为HyperText Markup Language，译为超文本标记语言，不是一种编程语言，是一种描述性的标记语言，用于描述超文本中内容的显示方式。比如字体什么颜色，大小等
- 超文本：超出文本的范畴，使用html可以轻松实现这样操作
- 标记：html所有的操作都是通过标记实现的，标记就是标签，<标签名称>
- Html就是超文本标记语言的简写，是最基础的网页语言
- Html是通过标签来定义的语言，代码都是由标签所组成

## html的规范（遵循）
1、一个html文件开始标签和结束的标签  `<html>...</html>`
2、html包含两部分内容 

- `<head>` 设置相关信息`</head>`
- `<body>` 显示在页面上的内容都写在body里面`</body>`

3、html的标签有开始标签，也要有结束标签
​      `<head></head>`
4、html的代码不区分大小写的
5、有些标签，没有结束标签 ，在标签内结束
​      比如 换行  `<br/>  <hr/>`

## html的操作思想
网页中有很多数据，不同的数据可能需要不同的显示效果，这个时候需要使用标签把要操作的数据包起来（封装起来），通过修改标签的属性值实现标签内数据样式的变化

一个标签相当于一个容器，想要修改容器内数据的样式，只需要改变容器的属性值，就可以实现容器内数据样式的变化

## html中常用的标签

- html：放在html文件的开头，但没有实质性的功能，即使没有这个标记，浏览器在碰到其他的html标记时也一样会进行解析。浏览器内置了html语言的解析器。
- head：头标记，放置关于此html文件的信息，如提供索引，定义css等。
- title：标题标记，包含在head标记内，它的作用是设定网页的标题。
- body：主体标记，网页所需要显示的内容都放在这个标记内。
- base：为页面上的所有链接规定默认地址或默认目标。 - 
- link：定义文档与外部资源的关系。

### 1. 文字标签和注释标签

文字标签：修改文字的样式，`<font color="red" size="5">文字</font>`

属性：

size: 文字的大小 取值范围 1-7，超出了7，默认还是7
color：文字颜色， 两种表示方式

- 英文单词：red  green  blue  black  white  yellow   gray......
- 使用十六进制数表示 #ffffff :  RGB
- 通过工具实现不同的颜色#66cc66


### 2. 标题标签、水平线标签和特殊字符

标题标签 

- `<h1></h1>  <h2></h2>  <h3></h3> ....... <h6></h6>`
- 从h1到h6，大小是依次变小，同时会自动换行

*  水平线标签
  - &lt;hr/>
  - 属性
  * size: 水平线的粗细 取值范围 1-7
  * color: 颜色
  - 代码
     &lt;hr size="5" color="blue"/>

* 特殊字符
  - 想要在页面上显示这样的内容 `<html> :是网页的开始！`
  - 需要对特殊字符进行转义
  * `< : &lt;`
  * `> : &gt;`
  * 空格：`&nbsp;`
  * &  :` &amp;`
  * 双引号”：`&quot;`
  * 注册符®：`&reg;`
  * 版权符©：` &copy;`

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	
	<!--  演示标题标签  -->
	<h1>标题一</h1>

	<h2>标题二</h2>

	<h3>标题三</h3>

	<h6>标题六</h6>

	<!--  演示水平线标签  -->
	<hr size="5" color="blue"/>

	<!-- 演示特殊字符 -->
	&lt;html&gt;:是&nbsp;&nbsp;&nbsp;&nbsp;网页的开始！

 </body>
</html>
```
## div

![html](http://img.blog.csdn.net/20161105194541948)

```html
<!DOCTYPE html>
<html>
    
    <head>
        <style type="text/css">
        div#container {
            width:500px
        }
        div#header {
            background-color:#99bbbb;
        }
        div#menu {
            background-color:#ffff99;
            height:200px;
            width:150px;
            float:left;
        }
        div#content {
            background-color:#EEEEEE;
            height:200px;
            width:350px;
            float:left;
        }
        div#footer {
            background-color:#99bbbb;
            clear:both;
            text-align:center;
        }
        h1 {
            margin-bottom:0;
        }
        h2 {
            margin-bottom:0;
            font-size:18px;
        }
        ul {
            margin:0;
        }
        li {
            list-style:none;
        }
        </style>
    </head>
    
    <body>
        <div id="container">
            <div id="header">
                 <h1>Main Title of Web Page</h1> 
            </div>
            <div id="menu">
                 <h2>Menu</h2> 
                <ul>
                    <li>HTML</li>
                    <li>CSS</li>
                    <li>JavaScript</li>
                </ul>
            </div>
            <div id="content">Content goes here</div>
            <div id="footer">Copyright W3School.com.cn</div>
        </div>
    </body>

</html>
```

## 列表标签

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<!-- 列表标签 -->
	<dl>
		<dt>传智播客</dt>
		<dd>财务部</dd>
		<dd>学工部</dd>
		<dd>人事部</dd>
	</dl>

	<hr/>

	<!-- 有序列表 -->
	<ol type="i">
		<li>财务部</li>
		<li>学工部</li>
		<li>人事部</li>
	</ol>

	<hr/>
	<!-- 无序列表 -->
	<ul type="square">
		<li>财务部</li>
		<li>学工部</li>
		<li>人事部</li>
	</ul>
 </body>
</html>
```
## 列表标签的案例

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<img src="images/header.jpg"/>
	<br/><br/>
	首页 > 中国馆 > 女装/女士精品 > 所有商品
	<br/><br/>
	<img src="images/list_header.jpg"/>
	<h1>热点推荐</h1>
	<dl>
		<dt><img src="images/photo_01.jpg"/></dt>
		<dd>一口价：49.00<br/>
全国包邮！韩版修身长袖T恤 打底衫 纯棉圆领T恤</dd>
	</dl>
	<img src="images/line.gif"/>
 </body>
</html>
```

## 图像标签

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>

	<img src="b1.jpg" alt="这是一个美女"/>

	<!-- <img src="w02.jpg" width="300" height="400" alt="这是图片上的文字"/>-->

	<img src="img\a1.jpg" alt="这是一个美女"/>

	<img src="../c.png" alt="这是一个美女"/>
 </body>
</html>

```
## 超链接标签
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	
	<a href="hello.html" target="_self">这是一个超链接2</a>

	<br/>
	<a href="#">这是一个超链接2</a>

	<br/>

	<pre>
	public static void main(String[] args) {
		System.out.println("hello");
	}
					aaaaaa
	</pre>
 </body>
</html>
```

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<a name="top">顶部</a>
	<pre>
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
天之道，损有余而补不足，是故虚胜实，不足胜有余。


其意博，其理奥，其趣深，天地之象分，阴阳之候列。

变化之由表，死生之兆彰，不谋而遗迹自同，

勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，

出不由产者亦。然刻意研精，探微索隐，或识契真要，则目牛无全，
故动则有成，犹鬼神幽赞，而命世奇杰，时时间出焉。
	</pre>
<a href="#top">回到顶部</a>
 </body>
</html>
```
## 表格标签
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
<!--
多少行： 4
每行里面有多少个单元格：3

-->
	<table border="1" bordercolor="blue" cellspacing="0" width="400" height="150">
		<caption>人员信息</caption>
		<tr align="center">
			<td>姓名</td>
			<td>性别</td>
			<td>年龄</td>
		</tr>
		<tr>
			<td align="right">东方不败</td>
			<td>男</td>
			<td>20</td>
		</tr>
		<tr>
			<td>岳不群</td>
			<td>女</td>
			<td>30</td>
		</tr>
		<tr>
			<th>林平之</th>
			<th>男</th>
			<th>40</th>
		</tr>
	</table>

 </body>
</html>
```

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>

<!--
   第一行：1
   第二行到第四行都是 3
 -->

 <table border="1" bordercolor="red" width="200">
 <tr>
	<td colspan="3">人员信息</td>
 </tr>
<tr>
	<td>东方不败</td>
	<td>男</td>
	<td>20</td>
</tr>
<tr>
	<td>岳不群</td>
	<td>女</td>
	<td>30</td>
</tr>
<tr>
	<td>林平之</td>
	<td>男</td>
	<td>40</td>
</tr>
 </table>

 <hr/>

 <!--多少行	： 3
数每行里面有多少个单元格
	第一个行：3
	第二行和第三行都是：2
-->
<table border="1" bordercolor="red" width="200" cellspacing="0">
<tr>
	<td>东方不败</td>
	<td>男</td>
	<td rowspan="3">20</td>
</tr>

<tr>
	<td>岳不群</td>
	<td>女</td>
</tr>

<tr>
	<td>林平之</td>
	<td>男</td>
</tr>
 </table>
 </body>
</html>
```
## 表单标签

- `<form>` 表单标签
  - action 提交的地址，默认是当前页面
  - method 提交方法，常用：get、post请求
- `<input>` 输入项标签，必须有name属性，该name值就是url后面的参数值
  - type 输入类型
    - text 文本
    - file 文件
    - password 密码
    - radio 单选，需要添加name属性且name的属性值必须相同，必须有value值
    - checkbox 复选（多选），需要添加name属性且name的属性值必须相同，必须有value值
    - submit 提交按钮，点击提交按钮后，表单中的数据会拼接在action地址的后面，然后提交到服务器
    - reset 重置按钮，回到输入项的初始状态
    - hidden 隐藏项
    - button 按钮，和js结合使用
    - image 使用图片提交表单
  - name，url参数中的name值
  - value，url参数中的value值
  - src
- select 下拉选择
  - option
- textarea 文本域
  - cols 列数
  - rows 行数

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>

	<form action="01-hello.html" method="get">
		手机号码：<input type="text" name="phone"/><br/>
		创建密码：<input type="password" name="pwd"/><br/>
		性别：<input type="radio" name="sex" value="nv" checked="checked"/>女 <input type="radio" name="sex" value="nan"/>男<br/>
		爱好：<input type="checkbox" name="love" value="y"/>羽毛球 <input type="checkbox" name="love" value="p"   checked="checked"/>乒乓球
			<input type="checkbox" name="love" value="pp"/> 排球<br/>
		文件：<input type="file"/><br/>
		生日：<select name="birth">
				<option value="0">请选择</option>
				<option value="1991" selected="selected">1991</option>
				<option value="1992">1992</option>
				<option value="1993">1993</option>
			</select>
		<br/>
		自我描述：<textarea cols="10" rows="10" name="tex"></textarea><br/>
		隐藏项：<input type="hidden" name="hid"/><br/>
		<input type="submit" value="注册"/>
		<input type="reset" value="重置注册"/>

		<input type="button" value="普通按钮"/>
		<br/>
		<!--  <input type="image" src="a.jpg"/>-->
	</form>
 </body>
</html>
```
案例 表单标签实现注册页面
```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>
	<h2>注册传智播客的账号</h2>
	<form action="01-hello.html">
	<table  width="100%">
		<tr>
			<td align="right">注册邮箱：</td>
			<td><input type="text" name="mail"/></td>
		</tr>
		<tr>
			<td>&nbsp;</td>
			<td>你可以使用 <a href="#">账号</a>注册或者使用 <a href="#">手机号</a>注册</td>
		</tr>
		<tr>
			<td align="right">创建密码：</td>
			<td><input type="password" name="pwd"/></td>
		</tr>
		<tr>
			<td align="right">真实姓名：</td>
			<td><input type="text" name="realname"/></td>
		</tr>
		<tr>
			<td align="right">性别：</td>
			<td><input type="radio" name="sex" value="nv"/>女 <input type="radio" name="sex" value="nan"/>男</td>
		</tr>

		<tr>
			<td  align="right" >生日：</td>
			<td>
				<select name="year">
					<option value="1945">1945</option>
					<option value="1931">1931</option>
					<option value="1949">1949</option>
				</select>年
				<select name="month">
					<option value="10">10</option>
					<option value="11">11</option>
					<option value="12">12</option>
				</select>月
				<select name="day">
					<option value="30">30</option>
					<option value="10">10</option>
					<option value="25">25</option>
				</select>日

			</td>
		</tr>

		<tr>
			<td  align="right">我现在：</td>
			<td>
				<select name="now">
					<option value="study">我正在上学</option>
					<option value="work">我已经工作</option>
				</select>

			</td>
		</tr>

		<tr>
			<td>&nbsp;</td>
			<td><img src="verycode.gif"/> <a href="#">看不清换一张?</a></td>
		</tr>

		<tr>
			<td align="right">验证码：</td>
			<td><input type="text" name="verycode"/></td>
		</tr>

		<tr>
			<td>&nbsp;</td>
			<td><input type="image" src="btn_reg.gif"/></td>
		</tr>
	</table>
</form>
 </body>
</html>
```
## 头标签

```html
<html>
 <head>
  <title>HTML示例</title>
  <meta name="keywords" content="毕姥爷，熊出没，刘翔">
   <!-- <meta http-equiv="refresh" content="3;url=01-hello.html" />-->
   <base target="_blank"/>
 </head>
 <body>

<h1>头标签</h1>

<a href="01-hello.html" >超链接1</a>
<a href="01-hello.html" >超链接2</a>
<a href="01-hello.html" >超链接3</a>
 </body>
</html>
```
## 框架标签

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
	<frameset rows="80,*">
			<frame name="top" src="a.html"/>

			<frameset cols="80,*">
				<frame name="left" src="b.html"/>
				<frame name="right"/>
			</frameset>
			
	</frameset>

</html>
```
## 其他常用的标签

```html
<html>
 <head>
  <title>HTML示例</title>
 </head>
 <body>

	<b>天之道</b>，
	<u>损有余而补不足</u>
	<i>是故虚胜实，</i>
	<s>不足胜有余。</s>

	<hr/>

	<pre>
		public static void main(String[] args) {
			System.out.println("hello");
		}
	</pre>

	<hr/>

	3 <sub>100</sub>

	<br/>
	4 <sup>200</sup>

	<hr/>
	<div>这是div1</div>
	<div>这是div2</div>
	<div>这是div3</div>

	<hr/>
	<span>这是span1</span>
	<span>这是span2</span>
	<span>这是span3</span>

	<hr/>

	<p>勿约而幽明斯契，稽其言有微，验之事不忒，诚可谓至道之宗，</p>
奉生之始矣。假若天机迅发，妙识玄通，成谋虽属乎生知，标格亦资于
治训，未尝有行不由送，
 </body>
</html>
```