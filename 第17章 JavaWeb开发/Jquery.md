为了简化 JavaScript 的开发, 一些 JavsScript 库诞生了. JavaScript 库封装了很多预定义的对象和实用函数。能帮助使用者建立有高难度交互的 Web2.0 特性的富客户端页面, 并且兼容各大浏览器

当前流行的 JavaScript 库有：jQuery，MooTools，Prototype，Dojo，YUI，EXT_JS  DWR

# **1. jquery是什么**

jQuery由美国人John Resig(莱西格)创建，至今已吸引了来自世界各地的众多javascript高手加入其team。

jQuery是继prototype之后又一个优秀的Javascript框架。其宗旨是——WRITE LESS,DO MORE,写更少的代码,做更多的事情。

它是轻量级的js库(压缩后只有21k) ，这是其它的js库所不及的，它兼容CSS3，还兼容各种浏览器 （IE 6.0+, FF 1.5+, Safari 2.0+, Opera 9.0+）。

jQuery是一个快速的，简洁的javaScript库，使用户能更方便地处理HTML documents、events、实现动画效果，并且方便地为网站提供AJAX交互。

jQuery还有一个比较大的优势是，它的文档说明很全，而且各种应用也说得很详细，同时还有许多成熟的插件可供选择。

jQuery能够使用户的html页保持代码和html内容分离，也就是说，不用再在html里面插入一堆js来调用命令了，只需定义id即可。

# **2. 什么是jQuery对象？**

jQuery 对象就是通过jQuery包装DOM对象后产生的对象。

jQuery 对象是 jQuery 独有的. 如果一个对象是 jQuery 对象, 那么它就可以使用 jQuery 里的方法: $(“#test”).html();
比如：

$("#test").html()   意思是指：获取ID为test的元素内的html代码。其中html()是jQuery里的方法

这段代码等同于用DOM实现代码：

```
document.getElementById(" test ").innerHTML;
```

虽然jQuery对象是包装DOM对象后产生的，但是jQuery无法使用DOM对象的任何方法，同理DOM对象也不能使用jQuery里的方法.乱使用会报错

约定：如果获取的是 jQuery 对象, 那么要在变量前面加上 $. 	

- var $variable = jQuery 对象
- var variable = DOM 对象

# **3. dom对象 -- Jquery对象**  
## **3.1 dom对象转为jquery对象**

jquery对象是\$()这样的基本形式，想要将dom对象转换为jquery对象，只需用$(dom对象)包一下就可以了，转换后就可以使用 jQuery 中的方法了

![jquery](http://img.blog.csdn.net/20161105235713735)

## **3.2 jquery对象转为dom对象**

可以像访问数组一样用[index]的方式将jquery对象转为dom对象

![jquery](http://img.blog.csdn.net/20161105235920991)

可以调用jquery对象的get(index)获取封装的dom对象  

![jquery](http://img.blog.csdn.net/20161105235949304)

# 4. 选择器
利用选择器jquery可以快速的在页面中选取节点

![jquery](http://img.blog.csdn.net/20161105115139740)
## 4.1 基本选择器
基本选择器是 jQuery 中最常用的选择器, 也是最简单的选择器, 它通过元素 id, class 和标签名来查找 DOM 元素(在网页中 id 只能使用一次, class 允许重复使用)

1、 #id     用法: \$(”#myDiv”);    返回值  单个元素的组成的集合

说明: 这个就是直接选择html中的id=”myDiv”

2、 Element       用法: $(”div”)     返回值  集合元素

说明: element的英文翻译过来是”元素”,所以element其实就是html已经定义的标签元素,例如 div, input, a等等.

3、 class          用法: \$(”.myClass”)      返回值  集合元素

说明: 这个标签是直接选择html代码中class=”myClass”的元素或元素组(因为在同一html页面中class是可以存在多个同样值的)

4、*          用法: \$(”*”)      返回值  集合元素

说明: 匹配所有元素,多用于结合上下文来搜索
​			 
5、selector1, selector2, selectorN      

用法: $(”div,span,p.myClass”)    返回值  集合元素

说明: 将每一个选择器匹配到的元素合并后一起返回.你可以指定任意多个选择器, 并将匹配到的元素合并到一个结果内.其中p.myClass是表示匹配元素p class=”myClass”

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value="改变 id 为 one 的元素的背景色为 #0000FF" id="b1" />
        <input type="button" value=" 改变元素名为 <div> 的所有元素的背景色为 #00FFFF" id="b2" />
        <input type="button" value=" 改变 class 为 mini 的所有元素的背景色为 #FF0033" id="b3" />
        <input type="button" value=" 改变所有元素的背景色为 #00FF33" id="b4" />
        <input type="button" value=" 改变所有的<span>元素和 id 为 two 的元素的背景色为 #3399FF" id="b5" />
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one</div>
        <div id="two" class="mini">id为two class是 mini
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
        <br>
        <div id="mover">动画</div>
        <br>	<span class="spanone">    span

		</span>
	<span class="mini">    span

		</span>
        <input type="text" value="zhang" id="username" name="username">
    </body>
    <script language="JavaScript">
    //<input type="button" value="改变 id 为 one 的元素的背景色为 #0000FF"  id="b1"/>
    $("#b1").click(function () {
        $("#one").css("background", "#0000FF");
    });
    //<input type="button" value=" 改变元素名为 <div> 的所有元素的背景色为 #00FFFF"  id="b2"/>
    $("#b2").click(function () {
        $("div").css("background", "#00FFFF");
    });
    //<input type="button" value=" 改变 class 为 mini 的所有元素的背景色为 #FF0033"  id="b3"/>
    $("#b3").click(function () {
        $(".mini").css("background", "#FF0033");
    });
    //<input type="button" value=" 改变所有元素的背景色为 #00FF33"  id="b4"/>
    $("#b4").click(function () {
        $("*").css("background", "#00FF33");
    });
    //<input type="button" value=" 改变所有的<span>元素和 id 为 two 的元素的背景色为 #3399FF"  id="b5"/>
    $("#b5").click(function () {
        $("span,#two").css("background", "#3399FF");
    });
    </script>

</html>
```


## 4.2 层次选择器
1 、ancestor descendant

用法: $(”form input”) ;   返回值  集合元素
说明: 在给定的祖先元素下匹配所有后代元素.这个要下面讲的”parent > child”区分开.
​			
2、parent > child用法: $(”form > input”) ;    返回值  集合元素

说明: 在给定的父元素下匹配所有子元素.注意:要区分好后代元素与子元素
​			
3、prev + next

用法: $(”label + input”) ;   返回值  集合元素
说明: 匹配所有紧接在 prev 元素后的 next 元素
​			
4、prev ~ siblings

用法: $(”form ~ input”) ;    返回值  集合元素
说明: 匹配 prev 元素之后的所有 siblings 元素.注意:是匹配之后的元素,不包含该元素在内,并且siblings匹配的是和prev同辈的元素,其后辈元素不被匹配.
​			
5、选择所有兄弟使用siblings方法来获取

```
$("#two").siblings("div").css();
```

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value="改变 <body> 内所有 <div> 的背景色为 #0000FF" id="b1" />
        <input type="button" value=" 改变 <body> 内子 <div> 的背景色为 #FF0033" id="b2" />
        <input type="button" value=" 改变 id 为 one 的下一个 <div> 的背景色为 #0000FF" id="b3" />
        <input type="button" value=" 改变 id 为 two 的元素后面的所有兄弟<div>的元素的背景色为 # #0000FF" id="b4" />
        <input type="button" value=" 改变 id 为 two 的元素所有 <div> 兄弟元素的背景色为 #0000FF" id="b5" />
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one</div>
        <div id="two" class="mini">id为two class是 mini
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
        <br>
        <div id="mover">动画</div>
        <br>	<span class="spanone">    span

		</span>
    </body>
    <script language="JavaScript">
    //<input type="button" value="改变 <body> 内所有 <div> 的背景色为 #0000FF"  id="b1"/>
    $("#b1").click(function () {
        $("body div").css("background", "#0000FF");
    });
    //<input type="button" value=" 改变 <body> 内子 <div> 的背景色为 #FF0033"  id="b2"/>
    $("#b2").click(function () {
        $("body>div").css("background", "#FF0033");
    });

    //<input type="button" value=" 改变 id 为 one 的下一个 <div> 的背景色为 #0000FF"  id="b3"/>
    $("#b3").click(function () {
        $("#one+div").css("background", "#0000FF");
    });
    //<input type="button" value=" 改变 id 为 two 的元素后面的所有兄弟<div>的元素的背景色为 # #0000FF"  id="b4"/>
    $("#b4").click(function () {
        $("#two ~ div").css("background", "#0000FF");
    });
    //<input type="button" value=" 改变 id 为 two 的元素所有 <div> 兄弟元素的背景色为 #0000FF"  id="b5"/>
    $("#b5").click(function () {
        $("#two").siblings("div").css("background", "#0000FF")
    });
    </script>

</html>
```

# 5. 过滤器
## 5.1 基础过滤选择器

![jquery](http://img.blog.csdn.net/20161105170355202)

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 改变第一个 div 元素的背景色为 #0000FF" id="b1" />
        <input type="button" value=" 改变最后一个 div 元素的背景色为 #0000FF" id="b2" />
        <input type="button" value=" 改变class不为 one 的所有 div 元素的背景色为 #0000FF" id="b3" />
        <input type="button" value=" 改变索引值为偶数的 div 元素的背景色为 #0000FF" id="b4" />
        <input type="button" value=" 改变索引值为奇数的 div 元素的背景色为 #0000FF" id="b5" />
        <input type="button" value=" 改变索引值为大于 3 的 div 元素的背景色为 #0000FF" id="b6" />
        <input type="button" value=" 改变索引值为等于 3 的 div 元素的背景色为 #0000FF" id="b7" />
        <input type="button" value=" 改变索引值为小于 3 的 div 元素的背景色为 #0000FF" id="b8" />
        <input type="button" value=" 改变所有的标题元素的背景色为 #0000FF" id="b9" />
        <input type="button" value=" 改变当前正在执行动画的所有元素的背景色为 #0000FF" id="b10" />
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one</div>
        <div id="two" class="mini">id为two class是 mini
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
        <br>
        <div id="mover">动画</div>
        <br>
    </body>
    <script language="JavaScript">
    //<input type="button" value=" 改变第一个 div 元素的背景色为 #0000FF"  id="b1"/>
    $("#b1").click(function () {
        $("div:first").css("background", "red");
    });

    //<input type="button" value=" 改变最后一个 div 元素的背景色为 #0000FF"  id="b2"/>
    $("#b2").click(function () {
        $("div:last").css("background", "red");
    });
    //<input type="button" value=" 改变class不为 one 的所有 div 元素的背景色为 #0000FF"  id="b3"/>
    $("#b3").click(function () {
        $("div:not(.one)").css("background", "red");
    });
    //<input type="button" value=" 改变索引值为偶数的 div 元素的背景色为 #0000FF"  id="b4"/>
    $("#b4").click(function () {
        $("div:even").css("background", "red");
    });
    //<input type="button" value=" 改变索引值为奇数的 div 元素的背景色为 #0000FF"  id="b5"/>
    $("#b5").click(function () {
        $("div:odd").css("background", "red");
    });
    //<input type="button" value=" 改变索引值为大于 3 的 div 元素的背景色为 #0000FF"  id="b6"/>
    $("#b6").click(function () {
        $("div:gt(3)").css("background", "red");
    });
    //<input type="button" value=" 改变索引值为等于 3 的 div 元素的背景色为 #0000FF"  id="b7"/>
    $("#b7").click(function () {
        $("div:eq(3)").css("background", "red");
    });
    //<input type="button" value=" 改变索引值为小于 3 的 div 元素的背景色为 #0000FF"  id="b8"/>
    $("#b8").click(function () {
        $("div:lt(3)").css("background", "red");
    });
    //<input type="button" value=" 改变所有的标题元素的背景色为 #0000FF"  id="b9"/>
    $("#b9").click(function () {
        $(":header").css("background", "red");
    });
    //<input type="button" value=" 改变当前正在执行动画的所有元素的背景色为 #0000FF"  id="b10"/>

    function move() {
        $("#mover").slideToggle("normal", move);
    }
    move();
    $("#b10").click(function () {
        $(":animated").css("background", "red");
    });
    </script>

</html>
```

## 5.2 内容过滤选择器

1、:contains(text)用法: \$(”div:contains(’John’)”)    返回值  集合元素

说明: 匹配包含给定文本的元素.这个选择器比较有用，当我们要选择的不是dom标签元素时,它就派上了用场了,它的作用是查找被标签”围”起来的文本内容是否符合指定的内容的.

2、:empty用法: \$(”td:empty”)   返回值  集合元素
说明: 匹配所有不包含子元素或者文本的空元素

3、:has(selector)

用法: \$(”div:has(p)”).addClass(”test”)    返回值  集合元素

说明: 匹配含有选择器所匹配的元素的元素.这个解释需要好好琢磨,但是一旦看了使用的例子就完全清楚了:给所有包含p元素的div标签加上class=”test”.

4、:parent用法: \$(”td:parent”)   返回值  集合元素

说明: 匹配含有子元素或者文本的元素.注意:这里是”:parent”,可不是”.parent”哦!感觉与上面讲的”:empty”形成反义词.

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 改变含有文本 ‘di’ 的 div 元素的背景色为 #0000FF" id="b1" />
        <input type="button" value=" 改变不包含子元素(或者文本元素) 的 div 空元素的背景色为" id="b2" />
        <input type="button" value=" 改变含有 class 为 mini 元素的 div 元素的背景色为 #0000FF" id="b3" />
        <input type="button" value=" 改变含有子元素(或者文本元素)的div元素的背景色为 #0000FF" id="b4" />
        <input type="button" value=" 改变不含有文本 di; 的 div 元素的背景色" id="b5" />
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one div</div>
        <div id="two" class="mini">id为two class是 mini div
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one"></div>
        <br>
        <div id="mover">动画</div>
        <br>
    </body>
    <script language="JavaScript">
    //<input type="button" value=" 改变含有文本 ‘di’ 的 div 元素的背景色为 #0000FF"  id="b1"/>
    $("#b1").click(function () {
        $("div:contains('di')").css("background", "red");
    });
    //<input type="button" value=" 改变不包含子元素(或者文本元素) 的 div 空元素的背景色为"  id="b2"/>
    $("#b2").click(function () {
        $("div:empty").css("background", "red");
    });
    //<input type="button" value=" 改变含有 class 为 mini 元素的 div 元素的背景色为 #0000FF"  id="b3"/>
    $("#b3").click(function () {
        $("div:has(.mini)").css("background", "red");
    });
    //<input type="button" value=" 改变含有子元素(或者文本元素)的div元素的背景色为 #0000FF"  id="b4"/>
    $("#b4").click(function () {
        $("div:parent").css("background", "red");
    });
    //<input type="button" value=" 改变不含有文本 di; 的 div 元素的背景色"  id="b5"/>
    $("#b5").click(function () {
        $("div:not(:contains('di'))").css("background", "red");
    });
    </script>

</html>
```

## 5.3 可见度过滤选择器
### :hidden用法: \$(”tr:hidden”)  返回值  集合元素
说明: 匹配所有的不可见元素，input 元素的 type 属性为 “hidden” 的话也会被匹配到.意思是css中display:none和input type=”hidden”的都会被匹配到.同样,要在脑海中彻底分清楚冒号”:”, 点号”.”和逗号”,”的区别.

### :visible用法: \$(”tr:visible”)  返回值  集合元素
说明: 匹配所有的可见元素

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        div.visible {
            display:none;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 改变所有可见的div元素的背景色为 #0000FF" id="b1" />
        <input type="button" value=" 选取所有不可见的元素, 利用 jQuery 中的 show() 方法将它们显示出来, 并设置其背景色为 #0000FF" id="b2" />
        <input type="button" value=" 选取所有的文本隐藏域, 并打印它们的值" id="b3" />
        <input type="button" value=" 选取所有的文本隐藏域, 并打印它们的值" id="b4" />
        <!--文本隐藏域-->
        <input type="hidden" value="hidden_1">
        <input type="hidden" value="hidden_2">
        <input type="hidden" value="hidden_3">
        <input type="hidden" value="hidden_4">
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one div</div>
        <div id="two" class="mini">id为two class是 mini div
            <div class="mini">class是 mini</div>
        </div>
        <div class="visible">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="visible">这是隐藏的</div>
        <div class="one"></div>
        <br>
        <div id="mover">动画</div>
        <br>
    </body>
    <script language="JavaScript">
    //<input type="button" value=" 改变所有可见的div元素的背景色为 #0000FF"  id="b1"/>
    $("#b1").click(function () {
        $("div:visible").css("background", "#0000FF")
    });
    //<input type="button" value=" 选取所有不可见的元素, 利用 jQuery 中的 show() 方法将它们显示出来, 并设置其背景色为 #0000FF"  id="b2"/>
    $("#b2").click(function () {
        $(":hidden").show();
    });
    //<input type="button" value=" 选取所有的文本隐藏域, 并打印它们的值"  id="b3"/>

    $("#b3").click(function () {

        $("input:hidden").each(function (i, obj) {
            alert(obj.value)
        });

        //		var length = $("input:hidden").length;
        //		for(var i = 0;i<length;i++){
        //			var obj = $("input:hidden")[i];
        //			alert(obj.value);
        //		}

    });
    </script>

</html>
```

## 5.4 属性过滤器
 1、[attribute]用法: \$(”div[id]“) ;  返回值  集合元素

 说明: 匹配包含给定属性的元素. 例子中是选取了所有带”id”属性的div标签.
 2、[attribute=value]用法: \$(”input[name='newsletter']“).attr(”checked”, true);    返回值  集合元素

说明: 匹配给定的属性是某个特定值的元素.例子中选取了所有 name 属性是 newsletter 的 input 元素.

3、[attribute!=value]用法: \$(”input[name!='newsletter']“).attr(”checked”, true);    返回值  集合元素

说明: 匹配所有不含有指定的属性，或者属性不等于特定值的元素.此选择器等价于:not([attr=value]),要匹配含有特定属性但不等于特定值的元素,请使用[attr]:not([attr=value]).之前看到的 :not 派上了用场.

4、[attribute^=value]用法: \$(”input[name^=‘news’]“)  返回值  集合元素

说明: 匹配给定的属性是以某些值开始的元素.,我们又见到了这几个类似于正则匹配的符号.现在想忘都忘不掉了吧?!

5、[attribute\$=value]用法: \$(”input[name\$=‘letter’]“)  返回值  集合元素

 说明: 匹配给定的属性是以某些值结尾的元素.

6、[attribute*=value]用法: \$(”input[name*=‘man’]“)   返回值  集合元素

 说明: 匹配给定的属性是以包含某些值的元素.

 7、[attributeFilter1][attributeFilter2][attributeFilterN]用法: \$(”input[id][name$=‘man’]“)  返回值  集合元素

说明: 复合属性选择器,需要同时满足多个条件时使用.又是一个组合,这种情况我们实际使用的时候很常用.这个例子中选择的是所有含有 id 属性,并且它的 name 属性是以 man 结尾的元素.



```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        div.visible {
            display:none;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 含有属性title 的div元素" id="b1" />
        <input type="button" value=" 属性title值等于test的div元素" id="b2" />
        <input type="button" value=" 属性title值不等于test的div元素(没有属性title的也将被选中)" id="b3" />
        <input type="button" value=" 属性title值 以te开始 的div元素." id="b4" />
        <input type="button" value=" 属性title值 以est结束 的div元素.." id="b5" />
        <input type="button" value="属性title值 含有es的div元素." id="b6" />
        <input type="button" value="选取有属性id的div元素，然后在结果中选取属性title值含有“es”的 div 元素" id="b7" />
        <!--文本隐藏域-->
        <input type="hidden" value="hidden_1">
        <input type="hidden" value="hidden_2">
        <input type="hidden" value="hidden_3">
        <input type="hidden" value="hidden_4">
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one div</div>
        <div id="two" class="mini" title="test">id为two class是 mini div title="test"
            <div class="mini">class是 mini</div>
        </div>
        <div class="visible">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one" title="test02">class是 one title="test02"
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="visible">这是隐藏的</div>
        <div class="one"></div>
        <br>
        <div id="mover">动画</div>
        <br>
        <input type="text" value="zhang" id="username" name="username">
    </body>
    <script language="JavaScript">
    //<input type="button" value=" 含有属性title 的div元素"  id="b1"/>
    $("#b1").click(function () {
        $("div[title]").css("background", "red")
    });
    //<input type="button" value=" 属性title值等于test的div元素"  id="b2"/>
    $("#b2").click(function () {
        $("div[title='test']").css("background", "red")
    });
    //<input type="button" value=" 属性title值不等于test的div元素(没有属性title的也将被选中)"  id="b3"/>
    $("#b3").click(function () {
        $("div[title!='test']").css("background", "red")
    });
    //<input type="button" value=" 属性title值 以te开始 的div元素."  id="b4"/>
    $("#b4").click(function () {
        $("div[title^='te']").css("background", "red")
    });
    //<input type="button" value=" 属性title值 以est结束 的div元素.."  id="b5"/>
    $("#b5").click(function () {
        $("div[title$='est']").css("background", "red")
    });
    //<input type="button" value="属性title值 含有es的div元素."  id="b6"/>
    $("#b6").click(function () {
        $("div[title*='es']").css("background", "red")
    });
    //<input type="button" value="选取有属性id的div元素，然后在结果中选取属性title值含有“es”的 div 元素"  id="b7"/>
    $("#b7").click(function () {
        $("div[id][title*='es']").css("background", "red")
    });
    </script>

</html>
```

## 5.5 子元素过滤选择器
1、:nth-child(index/even/odd/equation)

用法: \$(”ul li:nth-child(2)”)   返回值  集合元素

说明: 匹配其父元素下的第N个子或奇偶元素.这个选择器和之前说的基础过滤(Basic Filters)中的 eq() 有些类似,不同的地方就是前者是从0开始,后者是从1开始.


2、:first-child

用法: \$(”ul li:first-child”)    返回值  集合元素

说明: 匹配第一个子元素.’:first’ 只匹配一个元素,而此选择符将为每个父元素匹配一个子元素.这里需要特别点的记忆下区别.

3、:last-child

用法: \$(”ul li:last-child”)      返回值  集合元素

说明: 匹配最后一个子元素.’:last’只匹配一个元素,而此选择符将为每个父元素匹配一个子元素.

4、: only-child

用法: \$(”ul li:only-child”)   返回值  集合元素

说明: 如果某个元素是父元素中唯一的子元素,那将会被匹配.如果父元素中含有其他元素,那将不会被匹配.意思就是:只有一个子元素的才会被匹配!

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        div.visible {
            display:none;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 每个class为one的div父元素下的第2个子元素" id="b1" />
        <input type="button" value=" 每个class为one的div父元素下的第一个子元素" id="b2" />
        <input type="button" value=" 每个class为one的div父元素下的最后一个子元素" id="b3" />
        <input type="button" value=" 如果class为one的div父元素下的仅仅只有一个子元素，那么选中这个子元素" id="b4" />
        <!--文本隐藏域-->
        <input type="hidden" value="hidden_1">
        <input type="hidden" value="hidden_2">
        <input type="hidden" value="hidden_3">
        <input type="hidden" value="hidden_4">
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one div</div>
        <div id="one" class="mini" title="test">id为two class是 mini div title="test"
            <div class="mini">class是 mini</div>
        </div>
        <div class="one" title="test">
            <div class="mini">class是 mini******</div>
        </div>
        <div class="visible">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one" title="test02">class是 one title="test02" (**************
            <div class="mini01">class是 mini01</div>
            <div class="mini">cdddddddlass是 mini</div>
        </div>
        <div class="visible">这是隐藏的</div>
        <div class="one"></div>
        <br>
        <div id="mover">动画</div>
        <br>
    </body>
    <script language="JavaScript">
    /**注意：使用子元素过滤选择器的时候，要想体现父子关系，必须要在父子的中间添加【空格】或者【大于号】，这样才能体现父子关系，否则会有问题*/
    //<input type="button" value=" 每个class为one的div父元素下的第2个子元素"  id="b1"/>
    $("#b1").click(function () {
        $("div[class='one'] :nth-child(2)").css("background", "red");
    });
    //<input type="button" value=" 每个class为one的div父元素下的第一个子元素"  id="b2"/>
    $("#b2").click(function () {
        $("div[class='one'] :first-child").css("background", "red");
    });
    //<input type="button" value=" 每个class为one的div父元素下的最后一个子元素"  id="b3"/>
    $("#b3").click(function () {
        $("div[class='one'] :last-child").css("background", "red");
    });
    //<input type="button" value=" 如果class为one的div父元素下的仅仅只有一个子元素，那么选中这个子元素"  id="b4"/>
    $("#b4").click(function () {
        $("div[class='one'] :only-child").css("background", "red");
    });
    </script>

</html>
```

## 5.6 表单对象属性过滤选择器

![表单对象属性过滤选择器](http://img.blog.csdn.net/20161105170653169)

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内可用 <input> 元素的值" id="b1" />
        <input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内不可用 <input> 元素的值" id="b2" />
        <input type="button" value=" 利用 jQuery 对象的 length 属性获取多选框选中的个数" id="b3" />
        <input type="button" value=" 利用 jQuery 对象的 text() 方法获取下拉框选中的内容" id="b4" />
        <input type="text" value="不可用值1" disabled="disabled">
        <input type="text" value="可用值1">
        <input type="text" value="不可用值2" disabled="disabled">
        <input type="text" value="可用值2">
        <br>
        <input type="checkbox" name="items" value="美容">美容
        <input type="checkbox" name="items" value="IT">IT
        <input type="checkbox" name="items" value="金融">金融
        <input type="checkbox" name="items" value="管理">管理
        <br>
        <input type="radio" name="sex" value="男">男
        <input type="radio" name="sex" value="女">女
        <br>
        <select name="job" id="job" multiple="multiple" size=4>
            <option>程序员</option>
            <option>中级程序员</option>
            <option>高级程序员</option>
            <option>系统分析师</option>
        </select>
        <select name="edu" id="edu">
            <option>本科</option>
            <option>博士</option>
            <option>硕士</option>
            <option>大专</option>
        </select>
        <div id="two" class="mini">id为two class是 mini div
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
    </body>
    <script language="JavaScript">
    //<input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内可用 <input> 元素的值"  id="b1"/>
    $("#b1").click(function () {
        $("input[type='text']:enabled").val("xxxxxx");
    });
    //<input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内不可用 <input> 元素的值"  id="b2"/>
    $("#b2").click(function () {
        $("input[type='text']:disabled").val("zzzzzz");
    });
    //<input type="button" value=" 利用 jQuery 对象的 length 属性获取多选框选中的个数"  id="b3"/>
    $("#b3").click(function () {
        var length = $("input:checked").length;
        alert(length)
    });
    //<input type="button" value=" 利用 jQuery 对象的 text() 方法获取下拉框选中的内容"  id="b4"/>
    $("#b4").click(function () {
        $("select option:selected").each(function (i, obj) {
            var text = $(obj).text();
            alert(text)
        });
    });
    </script>

</html>
```

## 5.7 表单选择器

![表单选择器](http://img.blog.csdn.net/20161105170757388)

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" id="ok" />
        <input type="button" value=" 利用 jQuery 对象的 val()方法改变表单内可用<input>元素的值" id="b1" />
        <input type="button" value=" 利用 jQuery 对象的 val()方法改变表单内不可用<input>元素的值" id="b2" />
        <input type="button" value=" 利用 jQuery 对象的 length 属性获取多选框选中的个数" id="b3" />
        <input type="button" value=" 利用 jQuery 对象的 text() 方法获取下拉框选中的内容" id="b4" />
        <br>
        <input type="text" value="不可用值1" disabled="disabled">
        <input type="text" value="可用值1">
        <input type="text" value="不可用值2" disabled="disabled">
        <input type="text" value="可用值2">
        <br>
        <input type="checkbox" name="items" value="美容">美容
        <input type="checkbox" name="items" value="IT">IT
        <input type="checkbox" name="items" value="金融">金融
        <input type="checkbox" name="items" value="管理">管理
        <br>
        <input type="radio" name="sex" value="男" checked="checked">男
        <input type="radio" name="sex" value="女">女
        <br>
        <select name="job" id="job" multiple="multiple" size=4>
            <option value="程序员" selected="selected">程序员</option>
            <option value="中级程序员">中级程序员</option>
            <option value="高级程序员">高级程序员</option>
            <option value="系统分析师">系统分析师</option>
        </select>
        <select name="edu" id="edu">
            <option value="本科">本科</option>
            <option value="博士">博士</option>
            <option value="硕士">硕士</option>
            <option value="大专">大专</option>
        </select>
        <textarea cols=4 rows=4>textarea</textarea>
        <button value="确定">确定</button>
        <div id="two" class="mini">id为two class是 mini div
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini">class是 mini</div>
            <div class="mini">class是 mini</div>
        </div>
        <div class="one">class是 one
            <div class="mini01">class是 mini01</div>
            <div class="mini">class是 mini</div>
        </div>
    </body>
    <script language="JavaScript">
    //	<input type="button" value="保存"  class="mini" name="ok"  class="mini" id="ok" />
    // 点击OK时他，出所有输入框信息
    $("#ok").click(function () {
        $(":input").each(function (i, obj) {
            alert($(obj).val())
        })
    });
    </script>

</html>
```

# 6. Dom操作

## 6.1 内部插入
- append(content) :向每个匹配的元素的内部的结尾处追加内容
- appendTo(content) :将每个匹配的元素追加到指定的元素中的内部结尾处
- prepend(content):向每个匹配的元素的内部的开始处插入内容
- prependTo(content) :将每个匹配的元素插入到指定的元素内部的开始处

## 6.2 外部插入
- after(content) :在每个匹配的元素之后插入内容
- insertAfter(content):把所有匹配的元素插入到另一个、指定的元素元素集合的后面
- before(content):在每个匹配的元素之前插入内容
- insertBefore(content) :把所有匹配的元素插入到另一个、指定的元素元素集合的前面

## 6.3 查找节点:利用选择器就可以查找节点

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        div.visible {
            display:none;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 每个class为one的div父元素下的第2个子元素" id="b1" />
        <input type="button" value=" 每个class为one的div父元素下的第一个子元素" id="b2" />
        <input type="button" value=" 每个class为one的div父元素下的最后一个子元素" id="b3" />
        <input type="button" value=" 如果class为one的div父元素下的仅仅只有一个子元素，那么选中这个子元素" id="b4" />
        <!--文本隐藏域-->
        <input type="hidden" value="hidden_1">
        <input type="hidden" value="hidden_2">
        <input type="hidden" value="hidden_3">
        <input type="hidden" value="hidden_4">
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one div</div>
        <ul>
            <li id="bj" name="beijing">北京</li>
            <li id="tj" name="tianjin">天津</li>
        </ul>
    </body>
    <script language="JavaScript">
    //获取北京节点的name
    var name = $("#bj").attr("name");
    alert(name)
    //设置属性的值
    $("#bj").attr("name", "shoudu")
    var name = $("#bj").attr("name");
    alert(name)
    //删除name的属性值
    $("#bj").removeAttr("name");
    var name = $("#bj").attr("name");
    alert(name)
    </script>

</html>
```

## 6.4 创建节点:

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>

    <head>
        <title>ddd</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script language="JavaScript" src="../js/jquery-1.4.2.js"></script>
        <style type="text/css">
        div, span {
            width: 140px;
            height: 140px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float:left;
            font-size: 17px;
            font-family:Roman;
        }
        div.mini {
            width: 30px;
            height: 30px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family:Roman;
        }
        div.visible {
            display:none;
        }
        </style>
        <!--引入jquery的js库-->
    </head>

    <body>
        <input type="button" value="保存" class="mini" name="ok" class="mini" />
        <input type="button" value=" 每个class为one的div父元素下的第2个子元素" id="b1" />
        <input type="button" value=" 每个class为one的div父元素下的第一个子元素" id="b2" />
        <input type="button" value=" 每个class为one的div父元素下的最后一个子元素" id="b3" />
        <input type="button" value=" 如果class为one的div父元素下的仅仅只有一个子元素，那么选中这个子元素" id="b4" />
        <!--文本隐藏域-->
        <input type="hidden" value="hidden_1">
        <input type="hidden" value="hidden_2">
        <input type="hidden" value="hidden_3">
        <input type="hidden" value="hidden_4">
         <h1>天气冷了</h1>
         <h2>天气又冷了</h2>
        <div id="one">id为one div</div>
        <ul id="city">
            <li id="bj" name="beijing">北京</li>
        </ul>
    </body>
    <script language="JavaScript">
    //创建<li id="tj" name="tianjin">天津</li>的节点插入到北京的后面 $('<li id="tj" name="tianjin">天津</li>').insertAfter($("#bj"));
    //<li></li>
    var $li = $("<li></li>");
    //<li id="tj" name="tianjin">
    $li.attr("id", "tj");
    $li.attr("name", "tianjin");
    //<li id="tj" name="tianjin">天津</li>
    $li.text("天津");
    //将<li id="tj" name="tianjin">天津</li>的节点插入到北京的后面
    $li.insertAfter($("#bj"));
    </script>

</html>
```

## 6.5 删除节点:
remove()
empty()

## 6.6 克隆节点
clone()

## 6.7 替换节点
replaceWith
replaceAll

## 6.8 操作属性
attr()

# 7. jQuery 事件

![jQuery 事件](http://img.blog.csdn.net/20161105115307644)
