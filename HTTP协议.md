# HTTP协议
###### 客户端向服务器发送请求
无状态协议：第一次请求完成后断开，就算请求内容相同第二次请求也与第一次请求无关
###### 组成部分（三部分）
报文：HTTP协议的交互信息

请求行：包括请求方法、URL、HTTP协议版本

状态行：包括响应结果状态码、状态描述、HTTP版本

首部字段：包括请求和响应的各种条件和属性值（键值对）

报文首部、空行（CR+LF）、报文主体

报文首部：

请求：请求行、首部字段、其他

响应：状态行、首部字段、其他
##### 状态码

100-199 用于指定客户端应相应的某些动作。

200-299 用于表示请求成功。 

300-399 用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 

400-499 用于指出客户端的错误。 

500-599 用于支持服务器错误。 

100 (Continue/继续)

101 (Switching Protocols/转换协议)

200 (OK/正常)

201 (Created/已创建)

202 (Accepted/接受)

203 (Non-Authoritative Information/非官方信息)

204 (No Content/无内容)

205 (Reset Content/重置内容)

206 (Partial Content/局部内容)

300 (Multiple Choices/多重选择)

300 (SC_MULTIPLE_CHOICES)表示被请求的文档可以在多个地方找到，并将在返回的文档中列出来。如果服务器有首选设置，首选项将会被列于定位响应头信息中。 

301 (Moved Permanently)

301 (SC_MOVED_PERMANENTLY)状态是指所请求的文档在别的地方；文档新的URL会在定位响应头信息中给出。浏览器会自动连接到新的URL。 

302 (Found/找到)

与301有些类似，只是定位头信息中所给的URL应被理解为临时交换地址而不是永久的。注意：在 HTTP 1.0中，消息是临时移动(Moved Temporarily)的而不是被找到，因此HttpServletResponse中的常量是SC_MOVED_TEMPORARILY不是我们以为的SC_FOUND。 

304 (Not Modified/为修正)

当客户端有一个缓存的文档，通过提供一个 If-Modified-Since 头信息可指出客户端只希望文档在指定日期之后有所修改时才会重载此文档，用这种方式可以进行有条件的请求。304 (SC_NOT_MODIFIED)是指缓冲的版本已经被更新并且客户端应刷新文档。另外，服务器将返回请求的文档及状态码 200。servlet一般情况下不会直接设置这个状态码。它们会实现getLastModified方法并根据修正日期让默认服务方法处理有条件的请求。这个方法的例程已在2.8部分(An Example Using Servlet Initialization and Page Modification Dates/一个使用servlet初始化和页面修正日期的例子)给出。 

305 (Use Proxy/使用代理)

305 (SC_USE_PROXY)表示所请求的文档要通过定位头信息中的代理服务器获得。这个状态码是新加入 HTTP 1.1中的

307 (Temporary Redirect/临时重定向)

浏览器处理307状态的规则与302相同。307状态被加入到 HTTP 1.1中是由于许多浏览器在收到302响应时即使是原始消息为POST的情况下仍然执行了错误的转向。只有在收到303响应时才假定浏览器会在POST请求时重定向。添加这个新的状态码的目的很明确：在响应为303时按照GET和POST请求转向；而在307响应时则按照GET请求转向而不是POST请求。注意：由于某些原因在HttpServletResponse中还没有与这个状态对应的常量。该状态码是新加入HTTP 1.1中的。 

400 (Bad Request/错误请求)

400 (SC_BAD_REQUEST)指出客户端请求中的语法错误。 

401 (Unauthorized/未授权)

401 (SC_UNAUTHORIZED)表示客户端在授权头信息中没有有效的身份信息时访问受到密码保护的页面。这个响应必须包含一个WWW-Authenticate的授权信息头。例如，在本书4.5部分中的“Restricting Access to Web Pages./限制访问Web页。” 

403 (Forbidden/禁止)

403 (SC_FORBIDDEN)的意思是除非拥有授权否则服务器拒绝提供所请求的资源。这个状态经常会由于服务器上的损坏文件或目录许可而引起。 

404 (Not Found/未找到)

404 (SC_NOT_FOUND)状态每个网络程序员可能都遇到过，他告诉客户端所给的地址无法找到任何资源。它是表示“没有所访问页面”的标准方式。这个状态码是常用的响应并且在HttpServletResponse类中有专门的方法实现它：sendError("message")。相对于setStatus使用sendError得好处是：服务器会自动生成一个错误页来显示错误信息。但是，Internet Explorer 5浏览器却默认忽略你发挥的错误页面并显示其自定义的错误提示页面，虽然微软这么做违反了 HTTP 规范。要关闭此功能，在工具菜单里，选择Internet选项，进入高级标签页，并确认“显示友好的 HTTP 错误信息”选项（在我的浏览器中是倒数第8各选项）没有被选。但是很少有用户知道此选项，因此这个特性被IE5隐藏了起来使用户无法看到你所返回给用户的信息。而其他主流浏览器及IE4都完全的显示服务器生成的错误提示页面。

405 (Method Not Allowed/方法未允许)

405 (SC_METHOD_NOT_ALLOWED)指出请求方法(GET, POST, HEAD, PUT, DELETE, 等)对某些特定的资源不允许使用。该状态码是新加入 HTTP 1.1中的。 

406 (Not Acceptable/无法访问)

406 (SC_NOT_ACCEPTABLE)表示请求资源的MIME类型与客户端中Accept头信息中指定的类型不一致。见本书7.2部分中的表7.1(HTTP 1.1 Response Headers and Their Meaning/HTTP 1.1响应头信息以及他们的意义)中对MIME类型的介绍。406是新加入 HTTP 1.1中的。 

407 (Proxy Authentication Required/代理服务器认证要求)

407 (SC_PROXY_AUTHENTICATION_REQUIRED)与401状态有些相似，只是这个状态用于代理服务器。该状态指出客户端必须通过代理服务器的认证。代理服务器返回一个Proxy-Authenticate响应头信息给客户端，这会引起客户端使用带有Proxy-Authorization请求的头信息重新连接。该状态码是新加入 HTTP 1.1中的。 

408 (Request Timeout/请求超时)

408 (SC_REQUEST_TIMEOUT)是指服务端等待客户端发送请求的时间过长。该状态码是新加入 HTTP 1.1中的。 

409 (Conflict/冲突)

该状态通常与PUT请求一同使用，409 (SC_CONFLICT)状态常被用于试图上传版本不正确的文件时。该状态码是新加入 HTTP 1.1中的。 

410 (Gone/已经不存在)

410 (SC_GONE)告诉客户端所请求的文档已经不存在并且没有更新的地址。410状态不同于404，410是在指导文档已被移走的情况下使用，而404则用于未知原因的无法访问。该状态码是新加入 HTTP 1.1中的。 

411 (Length Required/需要数据长度)

411 (SC_LENGTH_REQUIRED)表示服务器不能处理请求（假设为带有附件的POST请求），除非客户端发送Content-Length头信息指出发送给服务器的数据的大小。该状态是新加入 HTTP 1.1的。 

412 (Precondition Failed/先决条件错误)

412 (SC_PRECONDITION_FAILED)状态指出请求头信息中的某些先决条件是错误的。该状态是新加入 HTTP 1.1的。 

413 (Request Entity Too Large/请求实体过大)

413 (SC_REQUEST_ENTITY_TOO_LARGE)告诉客户端现在所请求的文档比服务器现在想要处理的要大。如果服务器认为能够过一段时间处理，则会包含一个Retry-After的响应头信息。该状态是新加入 HTTP 1.1的。 

414 (Request URI Too Long/请求URI过长)

414 (SC_REQUEST_URI_TOO_LONG)状态用于在URI过长的情况时。这里所指的“URI”是指URL中主机、域名及端口号之后的内容。例如：在URL--http://www.y2k-disaster.com:8080/we/look/silly/now/中URI是指/we/look/silly/now/。该状态是新加入 HTTP 1.1的。 

415 (Unsupported Media Type/不支持的媒体格式)

415 (SC_UNSUPPORTED_MEDIA_TYPE)意味着请求所带的附件的格式类型服务器不知道如何处理。该状态是新加入 HTTP 1.1的。 

416 (Requested Range Not Satisfiable/请求范围无法满足)

416表示客户端包含了一个服务器无法满足的Range头信息的请求。该状态是新加入 HTTP 1.1的。奇怪的是，在servlet 2.1版本API的HttpServletResponse中并没有相应的常量代表该状态。 

注意
在servlet 2.1的规范中，类HttpServletResponse并没有SC_REQUESTED_RANGE_NOT_SATISFIABLE 这样的常量，所以你只能直接使用416。在servlet 2.2版本之后都包含了此常量。 

417 (Expectation Failed/期望失败)

如果服务器得到一个带有100-continue值的Expect请求头信息，这是指客户端正在询问是否可以在后面的请求中发送附件。在这种情况下，服务器也会用该状态(417)告诉浏览器服务器不接收该附件或用100 (SC_CONTINUE)状态告诉客户端可以继续发送附件。该状态是新加入 HTTP 1.1的。 

500 (Internal Server Error/内部服务器错误)

500 (SC_INTERNAL_SERVER_ERROR) 是常用的“服务器错误”状态。该状态经常由CGI程序引起也可能（但愿不会如此！）由无法正常运行的或返回头信息格式不正确的servlet引起。 

501 (Not Implemented/未实现)

501 (SC_NOT_IMPLEMENTED)状态告诉客户端服务器不支持请求中要求的功能。例如，客户端执行了如PUT这样的服务器并不支持的命令。 

502 (Bad Gateway/错误的网关)

502 (SC_BAD_GATEWAY)被用于充当代理或网关的服务器；该状态指出接收服务器接收到远端服务器的错误响应。 

503 (Service Unavailable/服务无法获得)

状态码503 (SC_SERVICE_UNAVAILABLE)表示服务器由于在维护或已经超载而无法响应。例如，如果某些线程或数据库连接池已经没有空闲则servlet会返回这个头信息。服务器可提供一个Retry-After头信息告诉客户端什么时候可以在试一次。 

504 (Gateway Timeout/网关超时)

该状态也用于充当代理或网关的服务器；它指出接收服务器没有从远端服务器得到及时的响应。该状态是新加入 HTTP 1.1的。 

505 (HTTP Version Not Supported/不支持的 HTTP 版本)

505 (SC_HTTP_VERSION_NOT_SUPPORTED)状态码是说服务器并不支持在请求中所标明 HTTP 版本。该状态是新加入 HTTP 1.1的。
好文要顶 关注我 收藏该文    

#### http和https

HTTPS（全称：Hypertext Transfer Protocol over Secure Socket Layer），是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。 它是一个URI scheme（抽象标识符体系），句法类同http:体系。用于安全的HTTP数据传输。https:URL表明它使用了HTTP，但HTTPS存在不同于HTTP的默认端口及一个加密/身份验证层（在HTTP与TCP之间）。这个系统的最初研发由网景公司进行，提供了身份验证与加密通讯方法，现在它被广泛用于万维网上安全敏感的通讯，例如交易支付方面。

超文本传输协议 (HTTP-Hypertext transfer protocol) 是一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议。

HTTPS和HTTP的区别：

https协议需要到ca申请证书，一般免费证书很少，需要交费。http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议http和https使用的是完全不同的连接方式用的端口也不一样,前者是80,后者是443。http的连接很简单,是无状态的HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议 要比http协议安全HTTPS解决的问题：1 . 信任主机的问题. 采用https 的server 必须从CA 申请一个用于证明服务器用途类型的证书. 改证书只有用于对应的server 的时候,客户度才信任次主机. 所以目前所有的银行系统网站,关键部分应用都是https 的. 客户通过信任该证书,从而信任了该主机. 其实这样做效率很低,但是银行更侧重安全. 这一点对我们没有任何意义,我们的server ,采用的证书不管自己issue 还是从公众的地方issue, 客户端都是自己人,所以我们也就肯定信任该server.2 . 通讯过程中的数据的泄密和被窜改1. 一般意义上的https, 就是 server 有一个证书.a) 主要目的是保证server 就是他声称的server. 这个跟第一点一样.b) 服务端和客户端之间的所有通讯,都是加密的.i. 具体讲,是客户端产生一个对称的密钥,通过server 的证书来交换密钥. 一般意义上的握手过程.ii. 加下来所有的信息往来就都是加密的. 第三方即使截获,也没有任何意义.因为他没有密钥. 当然窜改也就没有什么意义了.2. 少许对客户端有要求的情况下,会要求客户端也必须有一个证书.a) 这里客户端证书,其实就类似表示个人信息的时候,除了用户名/密码, 还有一个CA 认证过的身份. 应为个人证书一般来说上别人无法模拟的,所有这样能够更深的确认自己的身份.b) 目前少数个人银行的专业版是这种做法,具体证书可能是拿U盘作为一个备份的载体.HTTPS 一定是繁琐的.a) 本来简单的http协议,一个get一个response. 由于https 要还密钥和确认加密算法的需要.单握手就需要6/7 个往返.i. 任何应用中,过多的round trip 肯定影响性能.b) 接下来才是具体的http协议,每一次响应或者请求, 都要求客户端和服务端对会话的内容做加密/解密.i. 尽管对称加密/解密效率比较高,可是仍然要消耗过多的CPU,为此有专门的SSL 芯片. 如果CPU 信能比较低的话,肯定会降低性能,从而不能serve 更多的请求.ii. 加密后数据量的影响. 所以，才会出现那么多的安全认证提示

HTTPS网站对百度和谷歌SEO有什么影响？

从“site”中我们可以看见百度只收录http，尽管做了301跳转；谷歌方面则收录了2个不同版本的页面，很明确的指明了我的主域名是哪个版本。另外收录情况也是大大不用。再看看它们在搜索结果里面的情况。

google SERP 28

baidu SERP 500…

 现在就可以清楚的知道：https对google是没有丝毫影响的，不管是排名或者是收录。但是在baidu就明显行不通了，完全不收录https的站点，更别说排名。假如baidu没有发现你的http版本，那就是：抱歉，没有找到与“XX”相关的网页，就算是做了301，但是一个做了301的页面拿什么跟做了优化的对手网站竞争？

有时候一个网站因商业要求等先天条件必须要用到加密协议怎么办?

你主要市场的SE不支持https那一切都等于白搭了。所以最好清楚目标SE是什么态度，比如google那么你就可以不用理会了。

但是对于百度呢？

怎么处理或者避免这种情况发生置之不理。

一、直接复制一个http版本，https首页301到http

如一些特殊的网站，登陆后显示加密内容假如引用首页的话，可以在目录下复制一个首页，全部调用此目录，有需要可以在robots文件屏蔽掉。

二、站内外的链接一致采用http，有需要可以将之前的链接进行修改。

三、SE重新识别