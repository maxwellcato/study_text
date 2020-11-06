##### 附带

+ 源码：

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
      <style>
          table {
              width: 300px;
              height: 300px;
              border: 1px solid red;
              float: left;
          }
          tr{
              border: 1px solid blue;
          }
          td {
              border: 1px solid green
          }
      </style>
      
  </head>
  <body>
      <script>
  
          function createTable (tr){  //这个函数返回一个被参数挂载的table元素
              let table = (document.createElement('table'))
              table.appendChild(tr)
              return table
          }
          
          let tr = document.createElement('tr')
          let obj = {
              "0": null,
              "1": null,
              "2": null,
              "3": null,
          }
          for(let i = 0;i<4;i++) {
              let td = document.createElement('td')
              tr.appendChild(td)
              // console.log(tr)
              // console.log(i);
              // console.log('---------------------')
              console.log(tr)
              console.log(tr.childNodes)
              obj[i] = tr  //在循环中给obj的i属性赋值，按理说，应该是obj[0]中的tr有一个td，obj[1]中的tr有两个td，但是，输出的话，每一个obj[i]中都是四个td对象，而且鼠标浮上去时，都是触发的页面上的第四个table中内容
          }
  
          //console.log(obj);
          
          for(let i =0 ;i<4;i++){
              console.log(obj[i])
              let table = createTable(obj[i])
              document.body.appendChild(table)  //这里按理说，四个table元素分别应该包含1.2.3.4个td，然而，只有最后一个table中有四个td，前三个都是空的...
              // i===1?console.log(table):console.log('-------')
          }
      </script>
  </body>
  </html>
  
  //原因未知，可能涉及到了浏览器解析等方面的知识。
  

  <!-- 该问题已解决，详情查看该学习笔记内的js部分的浏览器渲染内容 -->
  ```
  
  

