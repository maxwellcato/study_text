##### 深拷贝的问题

+ 上代码：

  ```js
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
  
      let dots = {
        name: 'lr',
        age: 23,
        sex: null,
        val() {
  
        },
      }
  
      let dpc = new deepClone(dots)
  
      // dots.val = [1, 2, 3, 4];
  
      // console.dir(dots.val);
  
      // console.dir(dpc.val);
      
      console.dir(dpc)
  
      dots.val.success = 'hahahahahahahahahahaha'
  
      console.dir(dots.val);
  
      console.dir(dpc.val);
  
      console.dir(dots.val === dpc.val)
  
   //为啥深拷贝的出来的新对象里的属性会随着老对象的属性改变
  ```

  