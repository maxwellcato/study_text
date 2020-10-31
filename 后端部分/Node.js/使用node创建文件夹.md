### 使用node的 fs 创建文件夹



```javascript
//mkdir.js
const fs=require('fs');
const path = require('path')

const dirCache={};
function mkdir(file) {
    let filepath = path.join(__dirname,file)
    const arr=filepath.split('\\');//path模块处理过的路径间隔是用\来表示的，所以需要写两个，转义一下
    let dir=arr[0];
    for(let i=1;i<=arr.length;i++){
        if(!dirCache[dir]&&!fs.existsSync(dir)){
            dirCache[dir]=true;
            fs.mkdirSync(dir);
        }
        dir=dir+'/'+arr[i];
    }
}
module.exports=mkdir;
```

