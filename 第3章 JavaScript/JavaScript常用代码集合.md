> **手机类型判断**

```javascript
var BrowserInfo = {    
    userAgent: navigator.userAgent.toLowerCase()
    isAndroid: Boolean(navigator.userAgent.match(/android/ig)),
    isIphone: Boolean(navigator.userAgent.match(/iphone|ipod/ig)),
    isIpad: Boolean(navigator.userAgent.match(/ipad/ig)),
    isWeixin: Boolean(navigator.userAgent.match(/MicroMessenger/ig)),
}
```

> **返回字符串长度，汉子计数为2**

```javascript
function strLength(str) { 
    var a = 0;            
    for (var i = 0; i < str.length; i++) {
    if (str.charCodeAt(i) > 255)
    a += 2;//按照预期计数增加2
    else
    a++;
    }
    return a;
 }
```

> **获取url中的参数**

```javascript
function GetQueryStringRegExp(name,url) {
var reg = new RegExp("(^|\\?|&)" + name + "=([^&]*)(\\s|&|$)", "i"); 
if (reg.test(url)) 
    return decodeURIComponent(RegExp.$2.replace(/\+/g, " ")); 
}
```

> **js 绑定事件 适用于任何浏览器的元素绑定**

```javascript
function eventBind(obj, eventType, callBack) {
    if (obj.addEventListener) {
        obj.addEventListener(eventType, callBack, false);
    }else if (window.attachEvent) {
        obj.attachEvent('on' + eventType, callBack);
    }else {
        obj['on' + eventType] = callBack;
    }
};
eventBind(document, 'click', bodyClick);
```

> **获得当前浏览器JS的版本**

```javascript
function getjsversion(){    
    var n = navigator;    
    var u = n.userAgent;    
    var apn = n.appName;   
    var v = n.appVersion;    
    var ie = v.indexOf('MSIE ');    
    if (ie > 0){
        apv = parseInt(i = v.substring(ie + 5));        
        if (apv > 3) {
            apv = parseFloat(i);
        }
    } else {
        apv = parseFloat(v);
    }
    var isie = (apn == 'Microsoft Internet Explorer');    
    var ismac = (u.indexOf('Mac') >= 0);    
    var javascriptVersion = "1.0";    
    if (String && String.prototype) {
        javascriptVersion = '1.1';        
        if (javascriptVersion.match) {
            javascriptVersion = '1.2';            
            var tm = new Date;            
            if (tm.setUTCDate) {
                javascriptVersion = '1.3';                
                if (isie && ismac && apv >= 5) 
                javascriptVersion = '1.4';                
                var pn = 0;                
                if (pn.toPrecision) {
                    javascriptVersion = '1.5';
                    a = new Array;                    
                    if (a.forEach) {
                        javascriptVersion = '1.6';
                        i = 0;
                        o = new Object;
                        tcf = new Function('o', 'var e,i=0;try{i=new Iterator(o)}catch(e){}return i');
                        i = tcf(o);                        
                        if (i && i.next) {
                            javascriptVersion = '1.7';
                        }
                    }
                }
            }
        }
    }    
    return javascriptVersion;
}
```

> **获取当前点击事件的Object对象**

```javascript
function getEvent() {    
    if (document.all) {        
    return window.event; //如果是ie
    }
    func = getEvent.caller;    
    while (func != null) {        
    var arg0 = func.arguments[0];        
    if (arg0) {            
    if ((arg0.constructor == Event || arg0.constructor == MouseEvent)
|| (typeof (arg0) == "object" && arg0.preventDefault && arg0.stopPropagation)) {
            return arg0;
            }
        }
        func = func.caller;
    }    
    return null;
};
```

> **字符串截取方法**

```javascript
getCharactersLen: function (charStr, cutCount) {
    if (charStr == null || charStr == '') 
    return '';        
    var totalCount = 0;        
    var newStr = '';        
    for (var i = 0; i < charStr.length; i++) {            
    var c = charStr.charCodeAt(i);            
    if (c < 255 && c > 0) {
        totalCount++;
    } else {
        totalCount += 2;
    } 
    if (totalCount >= cutCount) {
        newStr += charStr.charAt(i);
        break;
    }else {
        newStr += charStr.charAt(i);
        }
   }        
   return newStr;
}
```

> **JS 弹出新窗口全屏**

```javascript
var tmp = window.open("about:blank", "", "fullscreen=1")
tmp.moveTo(0, 0);
tmp.resizeTo(screen.width + 20, screen.height);
tmp.focus();
tmp.location.href = 'http://www.che168.com/pinggu/eva_' + msgResult.message[0] + '.html';
var config_ = "left=0,top=0,width=" + (window.screen.Width) + ",height=" + (window.screen.Height);                            
window.open('http://www.che168.com/pinggu/eva_' + msgResult.message[0] + '.html', "winHanle", config_);
//模拟form提交打开新页面
var f = document.createElement("form");
f.setAttribute('action', 'http://www.che168.com/pinggu/eva_' + msgResult.message[0] + '.html');
f.target = '_blank';
document.body.appendChild(f);
f.submit();
```

> **全选/全不选**

```javascript
function selectAll(objSelect) {
    if (objSelect.checked == true) {
    $("input[name='chkId']").attr("checked", true);
    $("input[name='chkAll']").attr("checked", true);
    }else if (objSelect.checked == false) {
    $("input[name='chkId']").attr("checked", false);
    $("input[name='chkAll']").attr("checked", false);
    }
}
```

> **js 判断浏览器**

```javascript
判断是否是 IE 浏览器
if (document.all){ 
    alert(”IE浏览器”); 
}else{ 
    alert(”非IE浏览器”); 
} 
if (!!window.ActiveXObject){ 
    alert(”IE浏览器”); 
}else{ 
    alert(”非IE浏览器”); 
} 
判断是IE几
var isIE=!!window.ActiveXObject; 
var isIE6=isIE&&!window.XMLHttpRequest; 
var isIE8=isIE&&!!document.documentMode; 
var isIE7=isIE&&!isIE6&&!isIE8; 
if (isIE){ 
    if (isIE6){ 
        alert(”ie6″); 
    }else if (isIE8){ 
        alert(”ie8″); 
    }else if (isIE7){ 
        alert(”ie7″); 
    } 
}
```

> **判断浏览器**

```javascript
function getOs() {    
if (navigator.userAgent.indexOf("MSIE 8.0") > 0) {        
    return "MSIE8";
}else if (navigator.userAgent.indexOf("MSIE 6.0") > 0) {
    return "MSIE6";
}else if (navigator.userAgent.indexOf("MSIE 7.0") > 0) {
    return "MSIE7";
}else if (isFirefox = navigator.userAgent.indexOf("Firefox") > 0) {
    return "Firefox";
}
if (navigator.userAgent.indexOf("Chrome") > 0) {
    return "Chrome";
}else {
    return "Other";
    }
}
```

> **JS判断两个日期大小 **

```javascript
//得到日期值并转化成日期格式
//replace(/\-/g, "\/")是根据验证表达式把日期转化成长日期格式
//这样再进行判断就好判断了function ValidateDate() {
    var beginDate = $("#t_datestart").val();
    var endDate = $("#t_dateend").val();
    if (beginDate.length > 0 && endDate.length>0) {
        var sDate = new Date(beginDate.replace(/\-/g, "\/"));                
        var eDate= new Date(endDate.replace(/\-/g, "\/"));                
        if (sDate > eDate) {
            alert('开始日期要小于结束日期'); 
            return false;
        }
    }
}
```

> **移除事件**

```javascript
this.moveBind = function (objId, eventType, callBack) {
    var obj = document.getElementById(objId);
    if (obj.removeEventListener) {
        obj.removeEventListener(eventType, callBack, false);
    }else if (window.detachEvent) {
        obj.detachEvent('on' + eventType, callBack);
    }else {
        obj['on' + eventType] = null;
    }
}
```

> **回车提交**

```javascript
$("id").onkeypress = function (event) {    
    event = (event) ? event : ((window.event) ? window.event : "")
    keyCode = event.keyCode ? event.keyCode : (event.which ? event.which : event.charCode);    
    if (keyCode == 13) {
        $("SubmitLogin").onclick();
    }
}
```

> **JS 执行计时器**

```javascript
timeStart = new Date().getTime();
timesEnd = new Date().getTime();
document.getElementById("time").innerHTML = timesEnd - timeStart;
```

> **JS 写Cookie**

```javascript
function setCookie(name, value, expires, path, domain) {
    if (!expires) 
    expires = -1;
    if (!path) 
    path = "/";
    var d = "" + name + "=" + value;
    var e;
    if (expires < 0) {
        e = "";
    }else if (expires == 0) {
        var f = new Date(1970, 1, 1);
        e = ";expires=" + f.toUTCString();
    }else {
        var now = new Date();
        var f = new Date(now.getTime() + expires * 1000);
        e = ";expires=" + f.toUTCString();
    }
    var dm;
    if (!domain) {
        dm = "";
    }else {
        dm = ";domain=" + domain;
    }
    document.cookie = name + "=" + value + ";path=" + path + e + dm;
};
```

> **JS 读Cookie**

```javascript
function readCookie(name) {
    var nameEQ = name + "=";
    var ca = document.cookie.split(';');
    for (var i = 0; i < ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') c = c.substring(1, c.length);
        if (c.indexOf(nameEQ) == 0) {
            return decodeURIComponent(c.substring(nameEQ.length, c.length))
        }
    } 
    return null
}
```

> **Ajax 请求**

```javascript
C.ajax = function (args) {    var self = this;
    this.options = {
        type: 'GET',
        async: true,
        contentType: 'application/x-www-form-urlencoded',
        url: 'about:blank',
        data: null,
        success: {},
        error: {}
    };
    this.getXmlHttp = function () {
    var xmlHttp;
    try {
        xmlhttp = new XMLHttpRequest();
    }catch (e) {
        try {
            xmlhttp = new ActiveXObject("Msxml2.XMLHTTP");
        }catch (e) {
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
        }
    }if (!xmlhttp) {
        alert('您的浏览器不支持AJAX');
        return false;
    }
        return xmlhttp;
    };
    this.send = function () {
    C.each(self.options, function (key, val) {
    self.options[key] = (args[key] == null) ? val : args[key];
    });
    var xmlHttp = new self.getXmlHttp();
    if (self.options.type.toUpperCase() == 'GET') {
        xmlHttp.open(self.options.type, self.options.url + (self.options.data == null ? "" : ((/[?]$/.test(self.options.url) ? '&' : '?') + self.options.data)), self.options.async);
    }else {
        xmlHttp.open(self.options.type, self.options.url, self.options.async);
        xmlHttp.setRequestHeader('Content-Length', self.options.data.length);
    }
        xmlHttp.setRequestHeader('Content-Type', self.options.contentType);
        xmlHttp.onreadystatechange = function () {
        if (xmlHttp.readyState == 4) {
        if (xmlHttp.status == 200 || xmlHttp.status == 0) {                    if (typeof self.options.success == 'function') self.options.success(xmlHttp.responseText);
            xmlHttp = null;
        }else {
        if (typeof self.options.error == 'function') 
        self.options.error('Server Status: ' + xmlHttp.status);
                }
            }
        };
        xmlHttp.send(self.options.type.toUpperCase() == 'POST' ? self.options.data.toString() : null);
    };
    this.send();
};
```

> **JS StringBuilder 用法**

```javascript
function StringBuilder() {
this.strings = new Array;
};
StringBuilder.prototype.append = function (str) {
this.strings.push(str);
};
StringBuilder.prototype.toString = function () {
return this.strings.join('');
};
```

> **JS 加载到顶部LoadJS**

```javascript
function loadJS (url, fn) {
    var ss = document.getElementsByName('script'),
    loaded = false;
    for (var i = 0, len = ss.length; i < len; i++) {
        if (ss[i].src && ss[i].getAttribute('src') == url) {
            loaded = true;
            break;
            }
    }
    if (loaded) {
        if (fn && typeof fn != 'undefined' && fn instanceof Function) 
        fn();            
        return false;
    }
    var s = document.createElement('script'),
    b = false;
    s.setAttribute('type', 'text/javascript');
    s.setAttribute('src', url);
    s.onload = s.onreadystatechange = function () {
    if (!b && (!this.readyState || this.readyState == 'loaded' || this.readyState == 'complete')) {
        b = true; 
        if (fn && typeof fn != 'undefined' && fn instanceof Function) 
        fn();
        }
    };
    document.getElementsByTagName('head')[0].appendChild(s);
    },
bind: 
function (objId, eventType, callBack) {  //适用于任何浏览器的绑定
        var obj = document.getElementById(objId);        
        if (obj.addEventListener) {
            obj.addEventListener(eventType, callBack, false);
        }else if (window.attachEvent) {
            obj.attachEvent('on' + eventType, callBack);
        }else {
            obj['on' + eventType] = callBack;
        }
}
function JSLoad (args) { 
        s = document.createElement("script");
        s.setAttribute("type", "text/javascript");
        s.setAttribute("src", args.url);
        s.onload = s.onreadystatechange = function () {
    if (!s.readyState || s.readyState == "loaded" || s.readyState == "complete") {
        if (typeof args.callback == "function") 
            args.callback(this, args);
            s.onload = s.onreadystatechange = null;                
            try {
                s.parentNode && s.parentNode.removeChild(s);
                } catch (e) { }
            }
        };        
        document.getElementsByTagName("head")[0].appendChild(s);
    }
```

> **清空 LoadJS 加载到顶部的js引用**

```javascript
function ClearHeadJs  (src) { 
        var js = document.getElementsByTagName('head')[0].children;
        var obj = null;
        for (var i = 0; i < js.length; i++) {
        if (js[i].tagName.toLowerCase() == "script" && js[i].attributes['src'].value.indexOf(src) > 0) {
            obj = js[i];
           }
        }
        document.getElementsByTagName('head')[0].removeChild(obj);
};
```

> **JS 替换非法字符主要用在密码验证上出现的特殊字符**

```javascript
function URLencode(sStr) {
return escape(sStr).replace(/\+/g, '%2B').replace(/\"/g, '%22').replace(/\'/g, '%27').replace(/\//g, '%2F');
};
```

> **按Ctrl + Entert 直接提交表单**

```javascript
document.body.onkeydown = function (evt) {
        evt = evt ? evt : (window.event ? window.event : null);
        if (13 == evt.keyCode && evt.ctrlKey) {
            evt.returnValue = false;
            evt.cancel = true;
            PostData();
        }
};
```

> **获取当前时间**

```javascript
function GetCurrentDate() {
        var d = new Date();
        var y = d.getYear()+1900;
        month = add_zero(d.getMonth() + 1),
        days = add_zero(d.getDate()),
        hours = add_zero(d.getHours());
        minutes = add_zero(d.getMinutes()),
        seconds = add_zero(d.getSeconds());
        var str = y + '-' + month + '-' + days + ' ' + hours + ':' + minutes + ':' + seconds;        
        return str;
    }; 
function add_zero(temp) {
    if (temp < 10) return "0" + temp;
    else return temp;
}
```

> **Js 去掉空格方法**

```javascript
String.prototype.Trim = function(){ 
    return this.replace(/(^\s*)|(\s*$)/g, ""); 
    }
String.prototype.LTrim = function(){
    return this.replace(/(^\s*)/g, "");
    }
String.prototype.RTrim = function(){
    return this.replace(/(\s*$)/g, "");
    }
```

> **js 动态移除 head 里的 js 引用**

```javascript
this.ClearHeadJs = function (src) {
    var js = document.getElementsByTagName('head')[0].children;
    var obj = null;
    for (var i = 0; i < js.length; i++) {
    if (js[i].tagName.toLowerCase() == "script" && js[i].attributes['src'].value.indexOf(src) > 0) {
        obj = js[i];
    }
    }
    document.getElementsByTagName('head')[0].removeChild(obj);
};
```

> **整个URL 点击事件 加在URL里的onclick里**

```javascript
function CreateFrom(url, params) {
    var f = document.createElement("form");
    f.setAttribute("action", url);
    for (var i = 0; i < params.length; i++) {
         var input = document.createElement("input");
         input.setAttribute("type", "hidden");
         input.setAttribute("name", params[i].paramName);
         input.setAttribute("value", params[i].paramValue);
         f.appendChild(input);
    }
    f.target = "_blank";
    document.body.appendChild(f);            
    f.submit();
};
```

> **判断浏览器使用的是哪个 JS 版本**

```javascript
<script language="javascript">
      var jsversion = 1.0;    </script>
    <script language="javascript1.1">
      jsversion = 1.1;    </script>
    <script language="javascript1.2">
      jsversion = 1.2;    </script>
    ......
    ......
    <script language="javascript1.9">
      jsversion = 1.9;    </script>
    <script language="javascript2.0">
      jsversion = 2.0;    </script>
    alert(jsversion);
```