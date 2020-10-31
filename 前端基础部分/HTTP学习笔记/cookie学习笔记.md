笔记抄袭借鉴地址：https://www.bilibili.com/video/BV1mv411i7Tv?from=search&seid=696856119840620299

### 彻底搞懂cookie、session、token

#### cookie
+ 简介
    + 会话是由一组请求与响应组成，是围绕着一组相关事情所进行的请求与响应。所以这些请求与响应之间一定是需要有数据传递的，即是需要进行会话状态跟踪的。然而HTTP协议是一种无状态协议，在不同的请求间是无法进行数据传递的。此时就需要一种可以进行请求间数据传递的会话跟踪技术，而cookie就是这样的一种技术。
    + cookie是由服务器生成，保存在客户端的一种信息载体。这个载体中存放着用户访问该站点的会话状态信息。
    + 用户在提交第一次请求后，由服务器生成Cookie，并将其封装到响应头中，以响应的形式发送给客户端。客户端接收到这个响应后，将Cookie保存到客户端。当用户再次发送同类的请求后，在请求中会携带保存在客户端的Cookie数据，发送到服务端，由服务器对会话进行跟踪。
    + Cookie是由若干键值对构成，这里的键一般称为name，值称为value。Cookie中的键值均为字符串。
    + 不同的浏览器，其Cookie的保存位置及查看方式是不同的。删除了某一浏览器中的Cookie不会影响其他浏览器使用其保存的Cookie.


看到后边才发现，这个老师是讲Java的，算了，我一个前端的就不凑合了。
又找了一个：视频地址：https://www.bilibili.com/video/BV1ut411j7R7?from=search&seid=5149793963808484286
### cookie、localstorage、sessionstorage、session
+ 四种方式的共同点：都是键值对key-value存储，同域名可用，不可跨域
+ 存储位置：
	+ cookie、localstorage和sessionstorage都是客户端存储
	+ session是服务器端存储
+ 特点
	+ cookie：随请求头每次提交（比较消耗内存）
	+ localstorage：不随请求头提交，可长时间保存
	+ sessionstorage：不随请求头提交，页面关闭后即失效
	+ session：因为存储在服务器端，所以更为安全（session会为每一个访问key的浏览器为其分配一个浏览器ID保存在cookie中，请求数据时只需发送ID即可读取到数据，而且ID还可以自动改变，所以比较安全）
+ 跨页和跨域的问题：
	+ cookie、localstorage、session可跨页，不可跨域
	+ sessionstorage不可跨页，更不可跨域


