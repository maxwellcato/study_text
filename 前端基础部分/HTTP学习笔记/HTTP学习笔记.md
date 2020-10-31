笔记抄袭视频地址：https://www.bilibili.com/video/BV1o5411p7z4?from=search&seid=8007871196362385422
### URL&HTTP协议详解

#### URL:统一资源定位符，是通信访问常用的表现方式。
+ 示例：https://ke.qq.com/course/317690?tuin=15945f87
+ 通常来说，一个URL是由五个部分构成：protocol、domain、port、path、url parameters
#### protocol:协议，一般来说是位于url的最前方，位于字符串“://”之前。
+ 协议：通信双方对于所通信的数据所采用的的格式、规程、含义等所做的一个约定
+ 常见的应用层协议有：
	+ http
	+ https 	本质是http协议的过程中多了一个ssl协议
	+ ftp
	+ ssh
	+ smtp
	+ pop3
+ 数据库：
	+ oracle
	+ mysql
	+ MS SQL(sql server)
#### domain:域名，是“://”之后的部分，通常就是我们要访问的服务器名称、地址
+ 示例：
	+ www.baidu.com
	+ ke.qq.com
	+ 14.215.177.38
#### port:端口，是由服务器来配置指定的。端口是跟在域名之后的，格式一般为："domain:port"
+ 如果服务器所设置的端口是其应用协议的默认通信端口，则客户端在访问服务器时，可以省略端口。
+ 常见的协议及其默认通信端口的对应关系如下：
	+ http		80
	+ https		http+ssl 	443or8443
	+ ftp		21
	+ ssh		22
	+ smtp		25
	+ pop3		110
+ 数据库：
	+ oracle	1521
	+ mysql		3306
	+ MS SQL(sql server) 	1433
#### path:路径，通常来说就是指我们要访问的资源或者方法在服务器上的相对路径。路径一般都是相对于服务器的容器目录（根目录）而言。
#### url parameters: URL地址参数，通常就是以问号作为连接符连接在path之后，是用来传参的。数据格式为键值对方式，即key1=value&key2=value2...这样的格式。
+ PS：很多工具中，url parameters会直接合并到path部分，这是允许的。
#### http协议：超文本传输协议。其特点为：
+ http协议是一种基于request（请求）和response（响应）的协议。
+ http协议是一种简单、灵活的协议。
+ http协议是一种无连接的、快速的协议。
	+ 所谓的无连接可以理解为短连接。http1.0及以前，http协议默认是短连接的。即一个tcp连接只为一个http连接服务，http响应结束，下层的tcp连接也跟随关闭。
	+ http1.1开始，http协议默认是长连接的。
	+ 特征：在信息头中，通过Connection:key-alive来实现
	+ 一个tcp连接可以为多个http连接服务。（保持连接不挂断，从而为多个http连接服务）
+ http协议是一种无状态的协议。
	+ 随着web应用的发展，引入了session和cookie等机制来实现状态的记录。
#### http协议构成详解：
+ http协议是有两个部分构成：http request、http response。
#### http request: http请求，一般来说，http请求是由三个部分构成：request line、request header、request body
+ request line：请求行，是指请求数据包中的第一行。
	+ 示例：GET/phpwind/HTTP/1.1
+ 一般来说，包含以下信息：request method、request path、protocol/version
	+ request method:请求方法。一个http请求是必然会有请求方法，如果再发送是没有指定，默认使用的就是get。
	+ 常用的请求方法有：get、post、put、delete、patch、trace、options、header等等。
	+ request path：就是url中的path部分。包括url地址参数。
	+ protocol/version：协议和版本。
+ request header：请求头，是从数据包中的第二行开始到第一个空行结束（http协议的一个约定）
	+ 请求头是客户端用来告知服务器的一些控制、交互信息，通常来说和应用本身无关。
	+ 请求头的格式是键值对应的，格式为：key: values
	+ 请求头的名称（key）都是由协议规定的，是具有特定的含义和作用的。比较重要的有：
		+ User-Agent：用来告知服务器，客户端上的一些配置信息（环境信息）。通常该头和cookie、session的保持有关。（进行接口测试时，脚本中超过了一个请求，这种情况下大部分都要使用到客户端的session和cookie中的信息的，这就必须要添加这个key）
		+ Cookie：存放cookie信息，一般不建议手动添加（容易遇到过期的问题）。
		+ Content-Type：是客户端用来告知服务器，所发送的请求主体的数据组织格式
	+ request body：请求主体，即数据包中第一个空行之后的所有内容。通常就是接口向服务器发送的数据。PS：一旦body有数据，则信息头Content-Type一定要指定对应的值。
#### http response: http响应，是指服务器响应给客户端的。通常也是由三个部分构成：response line、response header、response body
+ response line： 响应行，即响应数据包中的第一行内容。通常包含以下信息：protocol/version、response code、response message
	+ 示例：HTTP/1.1 200 OK
    + protocol/version：协议和版本。
    + response code：响应代码，又叫状态码。是服务器用来告知客户端，服务器对于请求的逻辑处理状态。逻辑处理状态是指通信层面的，和业务无关。
        + 响应代码通常来说是由三维长度的数字构成，根据首位数字的不同，大致可以分为5类：
        + 1xx：表示通信建立过程中传递、控制、交互信息。
        + 2xx：典型的就是200，表示服务器处理成功。
        + 3xx：表示重定向。
        + PS：1xx、2xx、3xx都表示服务器是正常的，逻辑通信是成功的。
        + 4xx：表示客户端错误。
        + 5xx：表示服务器错误。
        + PS：一旦出现4xx和5xx，则表示客户端和服务器的通信出现了问题。
        + 在接口测试过程中，脚本运行一旦出现4xx和5xx，在排除服务器没有正常工作的前提下，都可以认为是我们的接口测试脚本处理有误导致。
        + 基本上所有的接口测试工具都会自动对状态码进行检测（不需要自己在手动单纯测试某个或某些状态码），不需要我们额外编写代码去进行检测。
	+ response message：响应信息，是用来描述响应代码，没有任何实际作用
+ response header：响应头，是指响应数据包中从第二行到第一个空行截止的部分
	+ 响应头类似于请求头，是服务器响应给客户端。
+ response body：响应主体，是指响应数据包中从第一个空行开始到最后的部分。
	+ 响应主体就是业务处理的结果。







