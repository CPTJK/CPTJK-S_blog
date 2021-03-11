# http cookie

## 概述

`HTTP cookie`(也叫`web cookie`或者`浏览器cookie`),是服务器发送到浏览器并且保存在本地的一小块数据,它会在浏览器下次像同一服务器再发起请求时被浏览器自动带上,通常,它用于告诉服务器两次请求是否来自同一浏览器,以便于保持用户的登录状态.**`cookie`使基于无状态的`http`协议记录稳定的状态信息成为可能**

`cookie`的作用主要可以总结为一下三个方面:

* 会话状态管理(如用户登录状态,购物车,游戏分数或者其它需要记录的数据)
* 个性化设置(如用户自定义设置,主题等)
* 浏览器行为跟踪(如跟踪分析用户行为等)

## 创建cookie

当服务器收到http请求时,服务器可以在响应头里面添加`set-cookie`字段.浏览器收到响应后通常会保存下`cookie`,之后对这个服务器的每一次请求中都通过`cookie`请求头部字段将`cookie`信息发送给服务器.

### set-cookie响应头部 和 cookie请求头部

服务器使用`set-cookie`响应头部向用户代理(一般指浏览器)发送`cookie`信息.一个简单的`cookie`像这个样子:
>Set-Cookie: <cookie名>=<cookie值>

看一个实际的例子:
>HTTP/1.0 200 OK
>Content-type: text/html
>Set-Cookie: yummy_cookie=choco
>Set-Cookie: tasty_cookie=strawberry
>
>[页面内容]

注意看上面**可以多次用`set-cookie`来设置多个`cookie`**

浏览器收到响应后,会根据响应头部将`cookie`信息保存起来(具体保存在哪里,后面说),下次再次向这个服务器发起请求的时候,浏览器就会自动在请求头部带上cookie信息,像这个样子:
>GET /sample_page.html HTTP/1.1
>Host: www.example.org
>Cookie: yummy_cookie=choco; tasty_cookie=strawberry

### 设定生命周期

`cookie`的生命周期可以通过两种方式定义:

* 会话期`cookie`:是最简单的`cookie`,浏览器关闭之后它会被自动删除,也就是说它尽在会话期内有效.会话期`cookie`不需要指定过期时间(`Expires`)或者有效期(`Max-Age`).需要注意的是,**有些浏览器提供了会话恢复功能,这种情况下即使关闭了浏览器,会话期cookie也会被保存下来,就好像浏览器从来没有关闭一样,这会导致cookie的生命周期无限延长**
* 持久性cookie: 生命周期取决于过期时间(`Expires`)或有效期(`Max-Age`)的设定

例如:
>Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
>**提示:** 当cookie的过期时间被设定时,设定的日期和时间只与客户端相关,而不是服务器

如果你的网站对用户进行身份验证,则每当用户进行身份验证时,它都应该重新生成并且重新发送会话`cookie`. 这样有助于防止`会话固定攻击(session fixation attacks)`,在该攻击中第三方可以重用用户的会话.

### 限制访问cookie

有两种方法可以确保cookie被安全发送,并且不会被意外的参与者或者脚本访问:`Secure`属性和`HTTPOnly`属性.
>Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly

标记`secure`的cookie只应通过HTTPS协议加密过的请求发送给服务端,因此可以预防`中间人攻击(man-in-the-middle)`.但是即便设置了`secure`标记,敏感信息也不应该通过`cookie`传输,因为`cookie`有其固有的不安全性,例如,可以访问客户端的硬盘的人就可以直接读取到cookie信息.

JavaScript 的 `Document.cookie`API 无法访问带有 `HTTPOnly`属性的`cookie`. `httpOnly`有助于预防XSS攻击.

### cookie的作用域

### Path
