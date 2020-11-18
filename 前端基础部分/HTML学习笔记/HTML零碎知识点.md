##### input单选框

+ input单选的效果通过把name属性设置为同样的值来实现。

+ 与之配套的label有两种方法来实现效果（label的效果是点击label标签，id值与label的for相同的input为选中状态）

  ```html
  1.<label><input value='one' type = "radio"></label>
  
  2.<input id="one" value='one' type = "radio" >
    <label for="one">第一个</label>
    <input id="two" value='two' type = "radio" >
    <label for="two">第二个</label>
  ```

+ 补充：

  + 在vue中，input中不写name的话，可以通过写v-model来实现同样的效果