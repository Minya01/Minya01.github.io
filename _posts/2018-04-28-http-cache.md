---
layout: post
title:  认识http缓存机制
date:   2018-04-28
categories: http
---

在开发微信公众号页面的时候，因为使用的是volecity模板开发，在前端设置禁止缓存的情况下，完全没起到作用。如何权衡该如何使用http cache？所以研究了下http缓存机制。

对于用户体验，网络不好时，等待时间越长体验越差。为了减少等待时间、节省流量，利用http缓存机制是不可避免的。通过了解其工作原理，可以减少延迟和带宽，提高用户体验。



### 什么是缓存

缓存无处不在。可以通过使用空间存储内容来节省时间。比如浏览器可以保存css、js、images以及整个页面。用户下一次进入该页面时，不比重新请求服务器，使得页面打开更快。

下图显示http的请求与响应：

![](/assets/images/HTTP_request.png)

> 步骤：
>
> * 浏览器请求服务器，询问是否有index.html
> * 服务器查找index.html
> * 服务器查到index.html，并拿给浏览器
> * 浏览器得到资源，加载显示给用户



## HTTP缓存头

#### 1.缓存控制

* 所有资源都可以通过HTTP头的Cache-Control的值不同定义缓存机制。
* 请求头和响应头都支持Cache-Control属性。

![](/assets/images/cache-control.jpg)



HTTP规范使服务器能够发送多个不同的缓存控制指令，这些缓存控制指令控制浏览器在单个请求响应中如何缓存多长时间、以及其他中间缓存（如CDN）是如何缓存的。

如下：

```http
Cache-control: private, max-age=0, no-cache
```

>上述指令表达：私有、最大缓存有效期=0、无缓存



##### public 和 private

即使在与HTTP身份验证相关联或HTTP响应状态码无法正常缓存的情况下，标记为`public`的响应也可以被缓存。通常，不需要标记为`public`，因为缓存信息（max-age)表明响应无论何时都可以被缓存的。

相反，标记为`private`的响应可以被浏览器缓存，但是这样的响应通常只针对单个用户，它们不能被中间缓存存储。（如：具有私有用户信息的html页面可以被用户浏览器缓存而不是CDN）。



##### no-cache 和 no-store

`no-cache` - 显示返回的响应在检查服务器响应是否发生变化之前无法用于对相同URL的后续请求。如果因此而出现适当的ETag（验证令牌），则no-cache会尝试往返以验证缓存的响应。但是，如果缓存没有改变，则不需要重新获取服务器内容。也就是说，web浏览器可能会缓存资源，如果资源发生了变化，则须检查每个请求（如果没有变化，则返回304响应）。



`no-store` - 不允许浏览器和所有的中间缓存存储任何返回响应的版本，即包含个人信息或隐私数据的响应。用户每次请求此资源时，都会讲请求发送到服务器，重新获取。

![](/assets/images/no-cache.jpg)



##### max-age

max-age之类声明允许再次使用缓存的最大时间（以发出请求的时间为单位）。如，`max-age=90`表示资源可以在接下来的90秒内不需要请求服务器，直接从浏览器缓存获取。



##### s-maxage

“s-”表示“共享缓存”。该指令明确用于其他中间缓存中的CDN。此指令覆盖max-age指令和expires标题字段（如果存在）。keyCDN也服从这个指令。

Cache-Control指令是HTTP/1.1规范的一部分，取代了用于指定响应缓存策略的expires。所有现代留恋其都支持Cache-COntrol。



#### Pragma

老的“pragma”指令大部分都被新的指令替代了。其中`pragma:no-cache`被新指令`cache-control:no-cache`替代，因此可以忽略，因为KeyCDN的服务器会忽略该请求头信息。



#### Expires

![](/assets/images/expires.jpg)

曾经，这是指定资源缓存何时到期的主要方式（HTTP/1.1标准已经弃用，cache-control为其替代方案）。expires只是一个日期时间戳。对于依然在使用老系统的用户来说，这是非常有用的。然而，cache-control、max-age和s-maxage在大多数现代化系统上更占优势。为了兼容性，给这个指令设置相应的值是一个好习惯，确保缓存时效。

```http
Expires: Sun, 03 May 2018 23:02:37 GMT
```



#### 验证器

##### ETag

![](/assets/images/etag.jpg)

关于此验证token（HTTP/1.1标准）：

* 通过ETag HTTP请求头（通过服务器）进行通信。
* 启用有效的资源更新，即如果资源不发生变更，则不会发生数据传输。

例如：（设置有效期为90秒）在第一次从服务器获取资源90秒后，重新发送一次完全相同的请求。浏览器查找本地缓存并找到以前的缓存响应，但是因为过期而无法使用。这是浏览器从服务器请求的完整内容。问题是，资源没有改变，那么就没有理由去服务器重新获取已经在CDN缓存中的相同资源。

验证token就可以解决上述问题。服务器创建并返回存储在请求头ETag字段中的任意标记，这些标记通常是现有文件的散列或其他内容的指纹。客户不需要知道如何生存此令牌，但需要知道在随后的请求中将它们发送到服务器。如果令牌（token）相同，则资源没有改变，不需要重新获取资源。

Web浏览器在”If-None-Match“HTTP请求头中自动提供ETag标记。服务器会检查缓存中当前资源的标记。如果服务器通过收到的标记判断缓存资源未变更，则发送`304 Not Modified`请求，节省带宽和时间。



##### Last-Modified

![](/assets/images/last-modified.jpg)

"Last-Modified"指示文档资源上次更改的时间。当缓存存储包含该指令的资源时，可以它已经超时（自上次出现），则可以利用它来查询服务器。这可以使用”If-Modified-Since“请求头字段来完成。

HTTP/1.1源服务器应该同时发送ETag和Last-Modified值。

KeyCDN响应头示例：

```http
HTTP/1.1 200 OK
Server: keycdn-engine
Date: Sat, 28 Apr 2018 10:44:16 GMT
Content-Type: text/css
Content-Length: 44660
Connection: keep-alive
Vary: Accept-Encoding
Last-Modified: Thu, 23 Feb 2017 08:44:33 GMT
ETag: "5485fac7-ae74"
Cache-Control: max-age=533280
Expires: Sat, 28 Apr 2018 10:49:16 GMT
X-Cache: HIT
X-Edge-Location: defr
Access-Control-Allow-Origin: *
Accept-Ranges: bytes
```

## 结论

在提高性能方面，充分利用HTTP缓存是创建响应速度更快的应用程序的最简单方法之一。理解HTTP请求头（响应头）是工作的第一步。























