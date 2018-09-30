---
title: HTTP请求响应关键词
date: 2018-03-12 21:37:43
tags: Http
---
首先这是一张http请求的图。
![image.png](http://upload-images.jianshu.io/upload_images/2088736-047733e4265975e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
http请求主要General、Response Header响应头、Request Header请求头、Query String Parameters参数

### General 一般的
Request URL 请求地址
Request Method 请求方式get/post/put/……
Status Code 状态码200 为请求成功
Remote Adderss 路由地址

### Response Header响应头
      1）Age：当代理服务器用自己缓存的实体去响应请求时，用该头部表明该实体从产生到现在经过多长时间了。
      2）Accept-Ranges：WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求。bytes：表示接受，none：表示不接受。
      3） Cache-Control：服务器应遵循的缓存机制。
              public(可以用 Cached 内容回应任何用户)
              private（只能用缓存内容回应先前请求该内容的那个用户）
              no-cache（可以缓存，但是只有在跟WEB服务器验证了其有效后，才能返回给客户端） 
              max-age：（本响应包含的对象的过期时间）  
              ALL:  no-store（不允许缓存）  
      4） Connection： 是否需要持久连接
              close（连接已经关闭）。
              keepalive（连接保持着，在等待本次连接的后续请求）。
              Keep-Alive：如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持连接多长时间（秒）。例如：Keep-Alive：300
      5）Content-Encoding：WEB服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。 例如：Content-Encoding：gzip 
      6）Content-Language：WEB 服务器告诉浏览器自己响应的对象的语言。
      7）Content-Length：WEB 服务器告诉浏览器自己响应的对象的长度。例如：Content-Length: 26012
      8）Content-Range：WEB 服务器表明该响应包含的部分对象为整个对象的哪个部分。例如：Content-Range: bytes 21010-47021/47022
      9）Content-Type：WEB 服务器告诉浏览器自己响应的对象的类型。例如：Content-Type：application/xml
     10）Expired：WEB服务器表明该实体将在什么时候过期，对于过期了的对象，只有在跟WEB服务器验证了其有效性后，才能用来响应客户请求。
     11） Last-Modified：WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间，动态页面的最后产生时间等等。
     12） Location：WEB 服务器告诉浏览器，试图访问的对象已经被移到别的位置了，到该头部指定的位置去取。
     13）Proxy-Authenticate： 代理服务器响应浏览器，要求其提供代理身份验证信息。
     14）Server: WEB 服务器表明自己是什么软件及版本等信息。
     15）Refresh：表示浏览器应该在多少时间之后刷新文档，以秒计。
     16）Transfer-Encoding：消息首部指明了将 [entity](https://developer.mozilla.org/en-US/docs/Glossary/Entity_header "entity: An entity header is an HTTP header describing the content of the body of the message. Entity headers are used in both, HTTP requests and responses. Headers like Content-Length, Content-Language, Content-Encoding are entity headers.") 安全传递给用户所采用的编码形式。
     17）X-Powered-By：含有模块modlayout/3.2.1 应该是1.3的apache 该模块的作用是能提供自动修改统一页头和页脚的指令。

### Request Header请求头
     1）Accept：  告诉WEB服务器自己接受什么介质类型，*/* 表示任何类型，type/* 表示该类型下的所有子类型；
     2）Accept-Charset：  浏览器申明自己接收的字符集
     Accept-Encoding：浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法  （gzip，deflate）
     3）Accept-Language：  浏览器申明自己接收的语言。语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等。
     4）Authorization：  当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，该头部来回应自己的身份验证信息给WEB服务器。
     5）Connection：表示是否需要持久连接。close（告诉WEB服务器或者代理服务器，在完成本次请求的响应后，断开连接，
     不要等待本次连接的后续请求了）。keep-alive（告诉WEB服务器或者代理服务器，在完成本次请求的响应后，保持连接，等待本次连接的后续请求）。
     6）Referer：发送请求页面URL。浏览器向 WEB 服务器表明自己是从哪个 网页/URL 获得/点击 当前请求中的网址/URL。
     7）User-Agent: 浏览器表明自己的身份（是哪种浏览器）。
     8）Host： 发送请求页面所在域。
     9）Cache-Control：浏览器应遵循的缓存机制。
            no-cache（不要缓存的实体，要求现在从WEB服务器去取）
            max-age：（只接受 Age 值小于 max-age 值，并且没有过期的对象） 
            max-stale：（可以接受过去的对象，但是过期时间必须小于 max-stale 值）  
            min-fresh：（接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的缓存对象）
     10）Pramga：主要使用 Pramga: no-cache，相当于 Cache-Control： no-cache。
     11）Range：浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分。
     12）Form：一种请求头标，给定控制用户代理的人工用户的电子邮件地址。
     13）Cookie：这是最重要的请求头信息之一
