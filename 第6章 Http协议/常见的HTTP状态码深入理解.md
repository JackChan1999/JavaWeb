状态码的职责是当客户端向服务器端发送请求时，描述返回请求结果。借助状态码，用户可以知道服务器端是正常处理了请求，还是出现了什么错误。

RFC2616定义的状态码，由3位数字和原因短信组成。

数字中的第一位指定了响应类别，后两位无分类。响应类别有以下5种：

| Type | Reason-phrase | Note                   |
| :--- | :------------ | :--------------------- |
| 1XX  | Informational | 信息性状态码，表示接受的请求正在处理     |
| 2XX  | Success       | 成功状态码，表示请求正常处理完毕       |
| 3XX  | Redirection   | 重定向状态码，表示需要客户端需要进行附加操作 |
| 4XX  | Client Error  | 客户端错误状态码，表示服务器无法处理请求   |
| 5XX  | Server Error  | 服务器错误状态码，表示服务器处理请求出错   |

RFC2616记录的HTTP状态码有37种，再加上「WebDAV」([RFC4918]()、[5842]())和「Additional HTTP Status Codes」([RFC6585]())，数量就达到60多种。

然并卵，这么多种HTTP状态码，其实常用的大概只有14种，本文就讲讲这14种状态码。

## 2XX Success

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjd2QvLicPXK0QicAupQPcbrrxOnRZaYGEDjIiaOiba88R5omaeCMtkGgyCkg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

2xx 响应结果表示从客户端发来的请求在服务器端被正常处理了。

## 200 OK

请求被成功处理，服务器会根据不同的请求方法返回结果：

- GET：请求的对应资源会作为响应返回。
- HEAD：请求的对应资源的响应头(entity-header)会作为响应返回,不包括响应体(message-body)。
- POST：返回处理对应请求的结果。

## 204 No Content

该状态码表示服务器接收到的请求已经处理完毕，但是服务器不需要返回响应体.

比如，客户端是浏览器的话，发出的请求返回204响应，那么浏览器显示的页面不会发生更新。

206 Partial Content

该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。

客户端发起的请求，必须在请求头中包含[Range]()字段。服务端响应报文中，必须包含由Content-Range指定范围的实体内容(entity-bodies )

## 3XX Redirection

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjd6BJnrDIicjZME615qtQeWnpWAojqNfZruNxWWCnicvKfd1PDkjFkmvibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

3XX 响应结果表明浏览器需要执行某些特殊的处理以完成请求。

## 301 Movied Permanently

永久性重定向。该状态码表示请求的资源已经被分配了新的URI，并且以后使用资源现在所指的URI。并且根据请求的方法有不同的处理方式：

- HEAD：必须在响应头部Location字段中指明新的永久性的URI。
- GET：除了有Location字段以外，还需要在响应体中附上永久性URI的超链接文本。
- POST：客户端在发送POST请求，受到301响应之后，不应该自动跳转URI，应当让用户确认跳转。

比如，如果一个URI已经在浏览器中被收藏为书签，这时应该按照Location首部字段提示的URI重新保存。

例如建立一个收藏的书签：

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjdffhKot0x2CunVicjgRqWTiajPWLDmsUfibEs3OaIcmYJQSMuaZa5HxQcg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

当访问这个书签的时候，请求会被重定向到

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjdAOFwSvQS1qU5UuGmYnbGsvglv1YaxAJ08zmhtAOdXXT6AzMuFfcyVA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

并且对应的书签会被改变，指向http://wan.meizu.com

不信？Try yourself.

## 302 Found

临时性重定向。该状态码表示请求的资源已被分配了新的URI，希望用户本次能使用新的URI访问。

和301 Moved Permanently 状态码相似，但302状态码代表的资源不是被永久移动，只是临时性质的。

如果，用户把一个URI收藏为书签，302响应是不会像301那样去更新书签。

## 303 See Other

该状态码表示由于请求对应的资源存在另一个URI，应使用GET方法定向获取请求的资源。303与302不同之处在于，302是不会改变请求的方法，如果请求方法是POST的话，重定向的请求也应该是POST。而对于303，使用POST请求的话，重定向的请求应该是GET请求。

但是有一点是需要注意的，许多HTTP/1.1版以前的浏览器不能正确理解303状态码，很多现存的浏览器讲302响应视为303响应，并且使用GET方式访问Location中规定的的URI，而无视原先请求的方法。

在RFC2616中有相关的这样一段原文：

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjduDg2aAiabLMfm7bKRjcicSlObicAP6W5mMULRGvOJJN618xZeeTbGVhtQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

## 304 Not Modified

该状态码表示客户端发送附带条件请求时，服务器端允许请求访问资源，但未满足条件的情况。304状态码返回时，不包含任何响应的主题部分。附带条件的请求指的是采用GET方法的请求头中包含：[If-Match]()、[If-Modified-Since]()、[If-None-Match]()、[If-Range]()、[If-Unmodified-Since]()中任一首部。

## 307Temporary Redirect

临时重定向。该状态码与302和303的有着类似的含义，不同之处在于，307状态码并不会指定客户端要用什么样的请求方法请求重定向地址。(302指定使用原有请求方法，303指定使用GET方法)

## 4XX Client Error

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjdEctibMUsf4xicTYiaicFgvprMVTVRtwSGgIaL7bBT2c7uG9s5wf97FK6Vg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

4XX 的响应结果表明客户端是发生错误的原因所在

## 400 Bad Request

表示该请求报文中存在语法错误，导致服务器无法理解该请求。客户端需要修改请求的内容后再次发送请求。

## 401 Unauthorized

该状态码表示发送的请求需要有通过HTTP认证(Basic认证，Digest认证)的认证信息。返回含有401的响应，必须在头部包含[WWW-Authenticate]()以指明服务器需要哪种方式的认证。

当客户端再次请求该资源的时候，需要在请求头中的[Authorization]()包含认证信息。

更多关于认证授权的信息关注RFC2617

## 403 Forbidden

该状态码表明对请求资源的访问被服务器拒绝了。服务器没有必要给出拒绝的详细理由，但如果想做说明的话，可以在实体的主体部分原因进行描述，这样就能让用户看到了。

未获得文件系统的访问权限，访问权限出现某些问题，从未授权的发送源IP地址试图访问等情况都可能发生403响应。

## 404 Not Found

该状态码表明服务器上无法找到指定的资源。通常被用于服务器不想透露拒绝请求的原因，或者没有其他的响应可提供。

## 5XX Server Error

![img](http://mmbiz.qpic.cn/mmbiz/EibqicLiaLZ06c0o1v7RJ2HvDLdosE2DJjdRGa1PhnxFd4FGBwKRegp9grrfhibojR6r2iaULcJicfAxZ9AZoXice5VYg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

5XX 的响应结果表明服务器本身发生错误，或者没有足够的能力来处理请求。

## 500 Internal Server Error

该状态码表明服务器端在执行请求时发生了错误。也有可能是Web应用存在的BUG或某些临时的故障。

## 503 Service Unavailable

该状态码表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。如果事先得知解除以上需要的时间，最好写入Retry-After首部字段再返回给客户端。