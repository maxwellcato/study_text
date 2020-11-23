#### 返回真正字符串长度的函数

```javascript
function codePointLength(text){
    let res = text.match(/[\S\s]/gu)
    // 因为''匹配不到东西，是null，这种情况是不能返回res.length的
    return res ? res.length:0
}
// 针对这种4位编码的Unicode，这种直接返回str.length的话，会返回8
let str = '𠮷𠮷𠮷𠮷'
```

#### 深克隆函数

```javascript
// function deepClone(obj) {
//     if(obj === null) return obj
//     if(typeof obj !== 'object') return obj
//     const cloneObj = new obj.constructor()
//     for(let i in obj) {
//         if(obj.hasOwnProperty(i)) {
//             cloneObj[i] = deepClone(obj[i])
//         }
//     }
//     return cloneObj
// }
function deepClone(obj) {
    if (obj === null) return obj
    const cloneObj = new obj.constructor()
    if (typeof obj !== 'object') return obj
    for (var i in obj) {
        if (obj.hasOwnProperty(i)) {
            cloneObj[i] = deepClone(obj[i])
        }
    }
    return cloneObj
}
```

