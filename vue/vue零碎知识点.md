##### 关于input框的value问题

+ 上代码：

  ```js
  <td>年龄：<input type="number" v-model='age'></td>
  <td><button @click='addStudent' :disabled="!infoComplete">创建新用户</button></td>
  ...
  data:{
      age:0
  }，
  updated(){
      this.checkIsComplete()
  }
  ...
  checkIsComplete(){
      if(this.name === '' || this.age === 0 || this.phone.length ===0){
          this.infoComplete = false
      }else{
          this.infoComplete = true
      }
  }
  ```

+ 问题描述：

  ```
  根据代码的逻辑判断，当age的输入框变成0时，data数据更新，运行判断函数，然后button应该变成无法点击。然而，当age输入框由0变成1的时候，button可以点击，但把1再变成0的时候，却不会变成不可点击状态，要再点击一下number框的减一才会变成不可点击状态
  ```

+ 解决办法

  ```shell
  把if的判断添加this.age===0变成Number(this.age)===0就好了
  改成this.age==0也可以
  #原因分析：
  因为页面刷新时,age的值为默认的给的number类型的0，但当我们在输入框中输入内容时，age就变成了了string类型，所以字符串的0和数字的0是不相等的，所以逻辑判断上出来问题(多点击一下可以的原因是，在watch中监听了age的变化，当age小于0时，会返回数字0，所以能成)
  age(){
      if(this.age>200){
          this.age = 200
      }else if(this.age<0){
      	this.age = 0
      }
  }
  ```


##### vue中的destroyed钩子函数中需要做的事情

```
如果项目中有使用settimeout或者setintervar的话，要在destroyed中清除计时器
```

