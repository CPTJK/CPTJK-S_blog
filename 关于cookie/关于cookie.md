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


## 生命周期

## 作用域

### domain

### Path
