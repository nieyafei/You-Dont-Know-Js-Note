# 循环和闭包

**首先看一个`for`循环**

```js
for(var i = 0;i<5;i++){
  setTimeout(function(){
    console.log(i);
  },1000)
}
```

?> 上面这个循环会打印出`5  5  5  5  5`,意外不？

**解析**

> - 其实for循环里面的变量`i`是`全局的变量`

> - for循环结束之后，`i`的值是5

> - `setTimeout`会在for循环结束之后执行

**针对上面的代码块，有什么方法能输出`0  1  2  3  4`?**

通过对闭包的了解，需要对代码块进行闭包作用域，每次的setTimeout都需要一个闭包作用域

```js
for(var i = 0;i<5;i++){
  (function(){
    setTimeout(function(){
      console.log(i);
    },1000)
  })()
}
```

这样的修改结果可能会让你失望，依然打印`5  5  5  5  5`,这是因为i这个变量依然是全局变量，而不是存在闭包作用域的变量，那么继续修改为闭包作用域的变量：

```js
for(var i = 0;i<5;i++){
  (function(){
    setTimeout(function(){
      var j = i;// 局部变量赋值
      console.log(j);
    },1000)
  })()
}
```

或者以参数的形式

```js
for(var i = 0;i<5;i++){
  (function(j){
    setTimeout(function(){
      console.log(j);
    },1000)
  })(i)
}
```

这样的修改是完全可以的，并且符合闭包的作用域

### 重返块作用域

```js
for(var i = 0;i<5;i++){
  let j = i;// 闭包的块作用域
  setTimeout(function(){
    console.log(j);
  },1000)
}
```
或者

```js
for(let i = 0;i<5;i++){
  setTimeout(function(){
    console.log(i);
  },1000)
}
```

?> `let`是ES6的声明变量

?> `let` 语句声明一个块级作用域的本地变量，并且可选的将其初始化为一个值

?> `let`允许你声明一个作用域被限制在块级中的变量、语句或者表达式。与`var`关键字不同的是，它声明的变量只能是全局或者整个函数块的。


