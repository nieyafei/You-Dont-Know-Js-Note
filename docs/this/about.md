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

**写段代码记录函数foo函数被调用的次数**

```js
function foo(num){
  console.log("foo：" + num);

  // 记录foo被调用的次数
  this.count++;
}

foo.count = 0;
var i;
for(i = 0;i<10;i++){
  if(i > 5){
    foo(i);
  }
}

console.log( foo.count );
```

**打印结果：**

```js
foo：6
foo：7
foo：8
foo：9
0  // 为什么是0？this.count的this指向的不是函数本身
```

**第一种修改方法：**

```js
function foo(num){
  console.log("foo：" + num);

  // 记录foo被调用的次数
  foo.count++;
}

foo.count = 0;
var i;
for(i = 0;i<10;i++){
  if(i > 5){
    foo(i);
  }
}

console.log( foo.count );
// 4
```

**第二种修改方法：**

```js
function foo(num){
  console.log("foo：" + num);

  // 记录foo被调用的次数
  this.count++;
}

foo.count = 0;
var i;
for(i = 0;i<10;i++){
  if(i > 5){
    foo.call(foo,i);// 使用call(..)可以确保指向函数对象foo对象
  }
}

console.log( foo.count );
// 4
```

## 它的作用域

?> `this`在任何情况下都不指向函数的词法作用域

```js
function foo(){
  var a = 2;
  this.bar();
}

function bar(){
  console.log(this.a);
}

foo();// a is not defined
```

?> `this`的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式

