## 关于this

?> `this`关键词是javascript中最复杂的机制之一

```js
function identify(){
  return this.name.toUpperCase();
}

function speak(){
  var greeting = "Hello " + identify.call(this)
  console.log(greeting);
}

var me = {
  name: "code"
}

var you = {
  name: "html"
}

identify.call(me);// CODE
identify.call(you);// HTML

speak.call(me);// Hello CODE
speak.call(you);// Hello HTML
```

> 这段代码可以在不同的上下文对象（me和you）中重复使用函数 `identify()`和`speak()`，不用针对每个对象编写不同版本的函数

## 指向自身


## 它的作用域
