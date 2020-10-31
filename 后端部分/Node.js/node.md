### 导入导出

##### 导入导出函数exports、module和require

+ 系统默认设置了：exports = module.exports，所以使用exports单独设置属性后再对module.exports赋值，覆盖掉之前对exports的赋值。（注意，只有赋值会覆盖掉exports的赋值，如果都是单独对属性进行设置，两种方法对同一属性赋值的话，谁在后面，以谁的赋值为准）
+ 导出时，注意使用exports时，只能单个设置属性exports.a = a，使用exports = {}的做法无效；而使用module.exports可以单个设置属性，也可以整个赋值
+ 建议使用module.exports = {}的方式来导出所需要的属性

##### 模块的初始化

+ 导入一个模块时，被导入的模块文件会被运行一次也只被运行一次。如果模块本身有console.log语句，那么单单被其他文件使用require('')语句导入模块就会执行模块中的console.log语句，从而产生输出。

##### 文件的读取和写入

+ 文件读取操作

  ```javascript
  //定义文件读取函数
  let fsRead = function(path) {
      return new Promise((resolve,reject) => {
          fs.readFile(path,{flag:'r',encoding: "utf-8"},(err,data)=> {
              if(err) {
                  reject(err)
              }else{
                  resolve(data)
              }
          })
      })
  };
  //使用
  fsRead('hello.txt').then(res => {
      console.log(res)
  }).catch(err => {
      console.log(err)
  })
  ```

  

+ 多次文件读取操作

  ```javascript
  async function ReadList() {
      var file2 = await fsRead('hello.txt');
      var file3 = await fsRead(file2.crim()+'.txt')//使用.crim()是因为文件后会默认加空格，需要把空格去掉
      var file3Content = await fsRead(file3.crim()+'.txt')
      console.log(file3Content)
  }
  ReadList();
  ```

+ 写入文件

  ```javascript
  function writefs(path,content){//使用异步写入文件，但是要在请求一个成功后再请求另一个，所以要用回调操作，为了避免回调地域，使用Promise构建函数
      return new Promise((resolve,reject) => {
          fs.writeFile(path,content,{flag:'a',encoding:'utf-8'},(err) => {
              if(err){
                  console.log('写入内容失败')
                  reject(err)
              }else{
                  resolve(content)
              }
          })
      })
  };
  async function writeList(){
      await writefs('test.txt','--今天吃烧烤\n')
      await writefs('test.txt','--今天还吃烧烤\n')
      await writefs('test.txt','--今天还吃烧烤吧\n')
      await writefs('test.txt','--今天还想吃烧烤\n')
  }
  writeList();
  //  writefs('test.txt','今晚吃啥？\n').then(res => {
  //     writefs('test.txt','胡辣汤\n')
  // }).then( res => {
  //     writefs('test.txt','我不会做\n');
  //     console.log('写入内容成功')
  // })
  ```

+ buffer

  ```javascript
  //数组不能进行二进制数据的操作
  //js数组效率较低
  //buffer内存空间开辟出固定大小的连续的内存
  
  //
  var str = 'hello'
  let buf = Buffer.from(str)
  //Buffer中存储字符串以16进制方式存储
  console.log(buf)// => <Buffer 68 65 6c 6c 6f>
  
  //可以使用toString()的方法将Buffer中的内容转换为字符串
  console.log(buf.toString())
  //开辟一个空的buffer(缓冲区)
  let buf1 = Buffer.alloc(10)
  buf1[1] = 255;//赋值256的话值会溢出，输出时只显示00
  console.log(buf1)
  ```
+ 读入目录fs.readdir(path,callback)
+ 删除目录fs.rmdir(path,callback)

##### 输入和输出

+ 输入

  + 首先导入readline包，并创建接口对象

    ```javascript
    let readline = require('readline');
    const { RSA_X931_PADDING } = require('constants');
    
    var rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout
    })
    //设置rl提问事件
    rl.question('去哪里工作',(answer) => {
        console.log('去'+answer)
        rl.close()
    })
    rl.on('close',() => {
        process.exit(0);
    })
    ```

##### 文件流

+ 写入流(正常的读写用flag，流式的读写用flags)

  ```javascript
  const fs = require('fs')
  //创建写入流
  //fs.createWriteStream(文件路径,[可选的配置操作])
  let ws = fs.createWriteStream('hello.txt',{flags:'w',encoding:'utf-8'})
  console.log(ws)
  
  //监听文件打开事件
  ws.on('open',() => {
      console.log('文件打开')
  })
  
  //监听准备时间
  ws.on('ready',() => {
      console.log('已准备写入文件')
  })
  
  ws.write('hello world',(err) => {
      if(err){
          console.log(err)
      }else{
          console.log('流入完成')
      }
  })
  ws.end(() => {console.log('文件写入关闭')})
  
  //监听文件关闭事件
  ws.on('close',() => {
      console.log('文件写入完成，关闭')
  })
  
  ```

##### node事件

+ 

##### 爬虫爬取内容

+ 1.爬虫介绍

  通过模拟浏览器的请求，服务器就会根据我们的请求返回我们想要的数据，将数据解析出来，并且进行保存

+ 2.爬虫流程

  + 1.目标：确定你想要获取的数据
    + 1.确定想要的数据在什么页面上（一般详细的数据会在详情页）
    + 2.确定在哪些页面可以链接到这些页面（一般分类列表页面会有详情页的链接数据）
    + 3.寻找页面之间和数据之间的规律
  + 2.-分析页面
    + 1.获取数据的方式（正则，cherrio）
    + 2.分析数据是通过ajax请求的数据，还是html里自带的数据
    + 3.如果是通过AJAX请求的数据，那么需要获取ajax请求的连接，一般请求到的数据都为JSON格式数据，那么就会比较容易解析
    + 4.如果数据在HTML里面，那么就用cherrio通过选择器将内容选中
  + 3-编写单个数据获取的案例
    + 1.解析出分类也的链接地址
    + 2.解析出列表页的链接地址
    + 3.解析出详情页的链接地址
    + 4.解析详情页里面想要获取的地质
    + 5.将数据进行保存到本地或者是数据库
  + 4-如果遇到阻碍进行反爬虫对抗
    + 1.User-Agent是否是正常浏览器的信息
    + 2.将请求头设置成跟浏览器一样的内容
    + 3.因为爬虫的爬取速度过快，会导致封号。所以
      + 1.通过降低速度来解决
      + 2.使用代理来解决
    + 4.如果设置需要凭证，那么可以采用无界浏览器真实模拟

+ 3.请求数据的库

  + axios，request

    + request：通过库，帮助我们快速实现HTTP请求包的打包

      ```javascript
      request.get('请求地址',{
      	'请求头字段': "请求头的value值"
      },(res)=>{在这里处理返回的内容})
      ```

    + axios优势会更明显，前后端均可用且调用方式相同

      ```javascript
      axios.get('请求地址',参数对象).then((response) => {
      	console.log(response.data)
      }).catch((err) => {
      	console.log(err)
      })
      ```

    + axios获取图片

      ```javascript
      axios({
      	method:'get',
      	url:'http://bit.ly/2mTTM3nY',
      	responseType:'Stream'
      }).then(response => {
      	response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
      })
      ```

      

  + **puppeteer(骨头浏览器)的无界面模式**完全模拟浏览器，防防爬虫性能好，但性能不好

    ```javascript
    //打开浏览器
    let options = {
    	headless: true，   //是否可视化
    	slowMo:250，  //调试时减慢操作速度
    	defaultViewport: {
    		width: 1200,  //设置视窗的宽高
    		height: 800
    	},
    	timeout: 3000  //默认超时3秒
    }
    let browser = await pupppeteer.launch().then(options);
    
    //打开新标签页
    let page = await browser.newPage()
    //获取浏览器中的所有界面
    let pages = await browser.pages()
    //关闭浏览器
    browder.close()
    //将页面跳转到
    await page.goto(url)
    //获取页面的对象，并进行操作
    let btn = page.$(selector)
    btn.click()  //点击按钮
    input.forcus()  //聚焦到输入框
    //在页面上写入内容
    await page.keyboard.type('Hello World');
    await page.keyboard.press('ArrowLEFT');
    await page.keyboard.down('Shift');
    //设置鼠标的移动
    await page.mouse.move(0,0);
    await page.mouse.down();
    await page.mouse.move(0,100);
    await page.mouse.move(100,100);
    await page.mouse.move(100,0);
    await page.mouse.move(0,0);
    await page.mouse.up();
    //截获页面请求（一般用于拦截谷歌浏览器中的广告）
    await page.setRequestInterception(true);
    page.on('request',request => {
        //request包含了所有请求信息	console.log(request.url)  //=>请求的网站 
        if(你想要的条件){
            request.continue()
        }else{
            request.abort([errorCode])
        }
    });
    //获取浏览器的信息和内容
    page.$eval(selector,(item) => {return item})
    page.$$eval(selectors,(items) => {return items})
    ```

##### 用于构造服务器的HTTP模块

+ 类型设置

  ```javascript
   设置状态码和响应头
      response.writeHead(200,{'Content-Type': 'text/plain'});
   设置响应头
      response.setHeader('Content-Type','text/html'); 
  ```

+ 最简单的服务器：

  ```javascript
  //http为内置模块，不需要安装
  let http = require('http')
  //创建server服务对象
  let server = http.createServer()
  //监听对当前服务器对象的请求
  server.on('request',(req,res) => {
      //当服务器被请求时，会触发请求事件，并传入请求对象和相应对象
      // console.log(req.url);
      // console.log(req.headers);
      res.setHeader('Content-Type','text/html; charset=UTF-8')
      if(req.url === '/'){
          res.end('首页')
      }else if(req.url === '/gnxw'){
          res.end('国内新闻')
      }else if(req.url === '/ylxw'){
          res.end('娱乐新闻')
      }else{
          res.end('404')
      }
  
  })
  
  //服务器监听的端口号
  server.listen(8000,() => {
      //启动监听端口号成功时触发
      console.log('服务器启动成功！')
  })
  ```

##### 服务器封装

+ 1.构造函数能够实例化app对象

+ 2.app.on(),可以添加路由的事件，根据请求的路径，去执行不同的内容

+ 3.app.run(port，callback)，让服务器运行起来

+ 目标

  ```javascript
  let app = new lcAPP()
  app.on('/',(req,res) => {
  	res.end('这是首页')
  })
  app.on('/gnxw',(req,res) => {
      res.end('gnwx')
  })
  app.run(80,() => {
      console.log('成功运行')
  })
  ```

+ 封装：

  ```javascript
  ja
  ```


##### 根据数据和模板动态生成页面

+ 1.根据规则去解析链接，并且获取ID或者是索引值

  ```javascript
  //请求路径：locahost/movies/0
  let index = req.pathObj.base;
  ```

+ 2.根据索引获取数据

  ```javascript
  let movies = [
      {
          name: '学霸',
          brief: '电影内容简介',
          author: '张震'
      },
      {
          name: '学渣',
          brief: '电影内容简介',
          author: '林伟祥'
      }
  ]
  let pageData = movies[index]
  ```

+ 3.根据模板渲染页面

  ```javascript
  res.render(movies[index],'./index.html')
  ```

+ 4.底层需要实现渲染函数(通过正则匹配，找到需要修改的地方进行逐一修改)

  ```javascript
  //模板渲染原理
  function render(options,path) {
      fs.readFile(path,{flag:'r',encoding:},(err,data) => {
          if(err) {
              console.log(err)
          }else{
              console.log(data);
              let reg = /\{\{(.*?)\}\}/igs
              let result;
              while(reslult = reg.exec(data)) {
                  let strKey = result[1].trim()
                  let strValue = options[strKey]
                  data = data.replace(result[0],strValue)
              }
              this.end(data)
          }
      })
  }
  ```

  






















