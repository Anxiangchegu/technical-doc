&emsp;&emsp;HTTP协议（HyperText Transfer Protocol，超文本传输协议）是用于从WWW服务器传输超文本到本地浏览器的传送协议。
## HTTP状态码:
&emsp;&emsp;HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型。HTTP状态码共分为5种类型：

分类 | 说明
---| ---
1** | 信息，服务器收到请求，需要请求者继续执行操作
2** | 成功，操作被成功接收并处理
3** | 重定向，需要进一步的操作以完成请求
4** | 客户端错误，请求包含语法错误或无法完成请求
5** | 服务器错误，服务器在处理请求的过程中发生了错误

##### 常见状态码：
- 200：请求成功；
- 301：资源（网页等）被永久转移到其他URL；
- 404：请求的资源（网页等）不存在；
- 500：内部服务器错误。

## HTTP头部信息
&emsp;&emsp;HTTP头部信息由众多的头域组成，每个头域由一个域名、冒号（：）和域值三部分组成。
##### 请求头信息

名称 | 说明
---| ---
GET | 请求方式，HTTP/1.1表示使用HTTP 1.1协议标准。
Host | 头域，用于指定请求资源的Intenet主机和端口号，必须表示请求URL的原始服务器或网关的位置。
User-Agent | 头域，里面包含发出请求的用户信息，其中有使用的浏览器型号、版本和操作系统的信息。这个头域经常用来作为反爬虫的措施。
Accept | 请求报头域，用于指定客户端接受哪些类型的信息。
Accept-Language | 请求报头域，类似于Accept，但是它用于指定一种自然语言。例如：Accept-Language：zh-cn。如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。
Accept-Encoding | 请求报头域，类似于Accept，但是它用于指定可接受的内容编码。例如：Accept-Encoding：gzip.deflate。如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。
Connection | 报头域允许发送用于指定连接的选项。例如指定连接的状态是连续，或者指定“close”选项，通知服务器，在响应完成后，关闭连接。
If-Modified-Since | 头域用于在发送HTTP请求时，把浏览器端缓存页面的最后修改时间一起发到服务器去，服务器会把这个时间与服务器上实际文件的最后修改时间进行比较。

##### 请求头信息

名称 | 说明
---| ---
HTTP/1.1 | 表示使用HTTP 1.1协议标准，200OK说明请求成功
Date | 表示消息产生的日期和时间。
Content-Type | 实体报头域用于指明发送给接收者的实体正文的媒体类型。text/html；charset=utf-8代表HTML文本文档，UTF-8编码。
Transfer-Encoding | chunked表示输出的内容长度不能确定。
Connection | 报头域允许发送用于指定连接的选项。例如指定连接的状态是连续，或者指定“close”选项，通知服务器，在响应完成后，关闭连接。
Vary | 头域指定了一些请求头域，这些请求头域用来决定当缓存中存在一个响应，并且该缓存没有过期失效时，是否被允许利用此响应去回复后续请求而不需要重复验证。
Cache-Control | 用于指定缓存指令，缓存指令是单向的，且是独立的。
Expires | 实体报头域给出响应过期的日期和时间。
Last-Modified | 实体报头域用于指示资源的最后修改日期和时间。
Content-Encoding | 实体报头域被用作媒体类型的修饰符，它的值指示了已经被应用到实体正文的附加内容的编码，因而要获得Content-Type报头域中所引用的媒体类型，必须采用相应的解码机制。

## Cookie状态管理
&emsp;&emsp;Cookie和Session都用来保存状态信息，都是保存客户端状态的机制，它们都是为了解决HTTP无状态的问题所做的努力。对于爬虫开发来说，我们更加关注的是Cookie，因为Cookie将状态保存在客户端，Session将状态保存在服务器端。  
&emsp;&emsp;Cookie是服务器在本地机器上存储的小段文本并随每一个请求发送至同一个服务器。网络服务器用HTTP头向客户端发送Cookie，浏览器则会解析这些Cookie并将它们保存为一个本地文件，它会自动将同一服务器的任何请求绑定上这些Cookie。  
&emsp;&emsp;Cookie的工作方式：服务器给每个Session分配一个唯一的JSESSIONID，并通过Cookie发送给客户端。当客户端发起新的请求的时候，将在Cookie头中携带这个JSESSIONID。

## HTTP请求方式

名称 | 说明
---| ---
GET | 请求指定的页面信息，并返回实体主体。
HEAD | 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头。
POST | 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
PUT | 从客户端向服务器传送的数据取代指定的文档的内容。
DELETE | 请求服务器删除指定的页面。
CONNECT | HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
OPTIONS | 允许客户端查看服务器的性能。
TRACE |	回显服务器收到的请求，主要用于测试或诊断。

- GET方式：是以实体的方式得到由请求URL所指定资源的信息，如果请求URL只是一个数据产生过程，那么最终要在响应实体中返回的是处理过程的结果所指向的资源，而不是处理过程的描述。
- POST方式：用来向目的服务器发出请求，要求它接受被附在请求后的实体，并把它当作请求队列中请求URL所指定资源的附加新子项。
- GET与POST方法区别：
     - 1、在客户端，GET方式通过URL提交数据，数据在URL中可以看到：POST方式，数据放置在实体区内提交。
     - 2、GET方式提交的数据最多只能有1024字节，而POST则没有此限制。
     - 3、安全性问题：使用Get的时候，参数会显示在地址栏上，而Post不会。所以，如果这些数据是非敏感数据，那么使用Get；如果用户输入的数据包含敏感数据，那么还是使用Post为好。  

> 在爬虫开发中基本处理的也是GET和POST请求。GET请求在访问网页时很常见，POST请求则是常用在登录框、提交框的位置。

##### HTTP头部信息实例
```
# General
Request URL: https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=%E7%88%AC%E8%99%AB&oq=%25E7%2588%25AC%25E8%2599%25AB&rsv_pq=a4f6a8af00015dde&rsv_t=b3d2fY1yGSvlQCB2PH5IJDQDZgjpjKoMhEbww2s%2BFSFWEnfVGYmcJc4agxg&rqlang=cn&rsv_enter=0
Request Method: GET
Status Code: 200 OK
Remote Address: 180.97.33.107:443
Referrer Policy: no-referrer-when-downgrade

# Response Headers
Bdpagetype: 3
Bdqid: 0x9c8e3d360001753d
Cache-Control: private
Ckpacknum: 2
Ckrndstr: 60001753d
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html;charset=utf-8
Date: Fri, 24 May 2019 03:30:41 GMT
Server: BWS/1.1
Set-Cookie: delPer=0; path=/; domain=.baidu.com
Set-Cookie: BD_CK_SAM=1;path=/
Set-Cookie: PSINO=3; domain=.baidu.com; path=/
Set-Cookie: BDSVRTM=13; path=/
Set-Cookie: H_PS_PSSID=1465_28940_21110_18560_29064_28518_29098_28721_28964_28835_28584; path=/; domain=.baidu.com
Strict-Transport-Security: max-age=172800
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Ua-Compatible: IE=Edge,chrome=1

# Request Headers
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Connection: keep-alive
Cookie: BAIDUID=7D57850671B605FA97FD63B8227A13D0:FG=1; PSTM=1557466905; BIDUPSID=CFD4FCAB8ED748AA84F97B7605F3469A; BD_UPN=12314753; H_WISE_SIDS=131797_125703_128700_131301_131784_130188_120187_131602_122157_131906_118886_118870_118856_118820_118805_130762_131651_131575_131535_131533_131529_130222_131295_131871_131391_129564_107311_131795_131392_130123_131874_130569_131194_130350_117437_131241_129648_131246_127025_131436_131688_131036_132091_130058_129900_129644_124030_131424_110085_127969_131506_123289_130819_131034_127417_131549_130597_131750_131264_131263_128600_131458_128808_131924_131958; Hm_lvt_12423ecbc0e2ca965d84259063d35238=1557976102,1557976146; delPer=0; BD_HOME=0; H_PS_PSSID=1465_28940_21110_18560_29064_28518_29098_28721_28964_28835_28584; BD_CK_SAM=1; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_PS_645EC=355aIKfcO8SaRRRkHWmqSzSx71ynZK0m0ynLchSojdXBVgn%2FrU7a3DCbT%2Bo; PSINO=6; BDSVRTM=112; COOKIE_SESSION=11_1_2_2_0_0_0_0_2_0_0_0_0_0_0_17_0_1558668598_1558668581%7C2%230_1_1558668581%7C1; shifen[100220636727_37639]=1558668597; BCLID=10200346891595581505; BDSFRCVID=a3KOJeC62xS7pnb9TUkTT9Gh_2VJyhoTH6aoSFO_AoVmhdBQuPVWEG0Pjx8g0Ku-Nb29ogKKXgOTHw0F_2uxOjjg8UtVJeC6EG0P3J; H_BDCLCKID_SF=tR4t_K0-fC03fP36q45H24k0-qrtetJyaR3KohvvWJ5TMCo90TJ2h4j-3boWLp083KbdofQFMRnoShPC-tn6Qx4mLfnp24vb0H6tbUQ63l02Vbc9e-t2ynQDXxKHq4RMW23v0h7mWP02sxA45J7cM4IseboJLfT-0bc4KKJxthF0HPonHjLBe5j-3H
Host: www.baidu.com
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36
```