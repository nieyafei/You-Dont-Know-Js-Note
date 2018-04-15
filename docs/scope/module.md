## 模块

**对于模块模式需要具备两个必要条件：**

?> 1、必须有外部的封闭函数，该函数必须至少被调用一次（每次调用都会创建一个新的模块实例）

?> 2、封闭函数必须返回至少一个内部函数，这样的内部函数才能在私有作用域中形成闭包，并且能访问或者修改私有的状态

```js
function module(){
  var arr1 = [1,2,3];
  var str = "cool";
  
  function doSomething(){
    console.log(arr1);
  }

  function doAnthor(){
    console.log(str);
  }
  return {
    doSomething: doSomething,
    doAnthor: doAnthor
  }
}

var foo = nodule();
foo.doSomething();
```

> `module()`是一个函数，调用之后形成一个模块实例，若不执行，则不会形成`内部作用域`和`闭包`的创建

> `module()`执行返回一个对象字面量的语法来表示对象，返回的对象中含有对内部函数而不是内部数据变量的引用

> 保持内部数据变量的隐藏且私有的状态

### 单例模式

```js
var foo = (function module(){
  var arr1 = [1,2,3];
  var str = "cool";
  
  function doSomething(){
    console.log(arr1);
  }

  function doAnthor(){
    console.log(str);
  }
  return {
    doSomething: doSomething,
    doAnthor: doAnthor
  }
})()// IIFE

foo.doSomething();
```

- **传递参数**

```js
function module(str){
  function identify(){
    console.log(str);
  }
  return {
    identify: identify
  }
}
var foo1 = module("nihao");
var foo2 = module("CODEHTML");
foo1.identify();// nihao
foo2.identify();// CODEHTMl
```

- **对内部模块实例进行修改方法和属性、值**

```js
var foo = (function(str){
  function thing1(){
    console.log("1."+str);
  }
  function thing2(){
    console.log("2."+str);
  }

  function change(){
    apiThing.thing = thing2;
  }
  var apiThing = {
    change: change,
    thing: thing1
  }
  return apiThing;
})("你好");

foo.thing();// 1.你好
foo.change();
foo.thing();// 2.你好
```

## 现代的模块机制

?> 调用包装了函数定义的包装函数，并且将返回值作为该模块的API

```js
var module = (function(){
  var modules = {};
  function define(name,deps,impl){
    for(var i = 0;i<deps.length;i++){
      deps[i] = modules[deps[i]];
    }
    modules[name] = impl.apply(impl,deps);
  }

  function get(name){
    return modules[name];
  }

  return {
    define: define,
    get: get
  }
})()
```

**如何使用这种模块机制呢？**

- 定义模块

```js
module.define('bar',[],function(){
  function hello(who){
    return "codehtml " + who
  }

  return {
    hello: hello
  }
})

module.define('foo',["bar"],function(bar){
  var hungry = "hippo";
  function awesome(){
    console.log(bar.hello("nieyafei"));
  }

  return {
    awesome: awesome
  }
})
```

- 使用模块

```js
var bar = module.get('bar');
var foo = module.get('foo');

console.log(bar.hello("xiaolizi"));// codehtml xiaolizi
foo.awesome();// codehtml nieyafei
```

## 未来模块机制

?> `ES6`增加了一级语法支持,`ES6`的模块没有`"行内"`格式，必须被定义在独立的文件中（一个文件一个模块）

```js
// bar.js

function hello(who){
  console.log(who);
}

export hello;
```

```js
// foo.js

import hello from "bar";

var hungry = "CODEHTML";

function awesome(){
  console.log(hello(hungry));
}

export awesome;
```

```js
// 导入上面两个模块

module foo from "foo";
module bar from "bar";

console.log(bar.hello());
foo.awesome();
```
