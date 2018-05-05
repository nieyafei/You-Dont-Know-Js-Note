# this的全面解析

## 调用位置

?> `调用位置：`调用位置就是函数在代码中被调用的位置（而不是声明的位置）

#### 分析调用栈和调用位置

```js
function baz(){
  // 当前调用栈： baz，因此当前的位置是全局作用域
  console.log("baz");

  bar();// bar的调用位置
}

function bar(){
  // 当前调用栈： baz --> bar，因此当前的位置是baz中
  console.log("bar");
  foo();// foo的调用位置
}

function foo(){
  // 当前调用栈： baz --> bar --> foo，因此当前的位置是bar中

  console.log("foo");
}

baz();// baz的调用位置
```

## 绑定规则

### 1、默认绑定

?> 最常用的函数调用类型：独立函数调用

```js
function foo(){
  console.log(this.a);// 此时的this指向全局对象，也就是默认的绑定
}

var a = 2;
foo();
```

**如果使用严格模式（strict mode），则不能将全局对象用于默认对象，此时的this会绑定到`undefined`**

- **使用严格模式绑定**

```js
function foo(){
  “use strict”;
  console.log(this.a);
}

var a = 2;
foo();// TypeError: this is undefined
```

- **使用严格模式调用**

```js
function foo(){
  console.log(this.a);
}

var a = 2;
(function(){
  foo();// 2
})()
```

### 2、隐式绑定

?> 调用的位置是否有上下文对象, **当函数引用有上下午对象时，`隐式绑定规则`会把函数调用中的this绑定到这个上下文对象**

```js
function foo(){
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
}

obj.foo();// 2
```

**注意：this绑定会被`隐式绑定`的函数丢失绑定对象, 也就是说会用`默认绑定`，从而把this绑定到全局对象或undefined上，取决于是否是严格模式**

```js
function foo(){
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
}

var bar = obj.foo;// 引用的是foo本身
var a = "code html";

bar();// code html
```

**参数传递的隐式赋值**

```js
function foo(){
  console.log(this.a);
}

function doFoo(fn){
  fn();
}

var obj = {
  a: 2,
  foo: foo
}

doFoo(obj.foo);

```

### 3、显式绑定

- #### call、apply

?> call、apply的方法的第一个参数是绑定到this的，也就是所说的`显示绑定`

```js
function foo(){
  console.log(this.a);
}

var obj = {
  a: 2
}

foo.call(obj);// 2
foo.apply(obj);// 2
```

> **如果你传入的是原始值（原始值类型、布尔类型、数字类型）来当做`this`绑定对象，原始值会被转换成它的对象形式,通常称为`装箱`**

> **new String(...)**

> **new Boolean(...)**

> **new Number(...)**

**使用硬绑定来解决丢失绑定的问题**

- #### 1、硬绑定

```js
function foo(){
  console.log(this.a);
}

var obj = {a: 2}

var bar = function(){
  foo.call(obj);// 硬绑定
}

bar();// 2
setTimeout(bar,100);// 2

// 硬绑定的bar不可以再修改它的this
var.call(window);// 2

```

**典型应用场景，创建一个包裹函数，负责接收参数并且返回值**

```js
function foo(something){
  console.log(this.a,something);
  return this.a + something;
}

var obj = {a:2}
var bar = function(){
  return foo.apply(obj,arguments);
}

var b = bar(3);// 2  3
console.log(b);// 5
```

**重复使用的辅助函数**

```js
function foo(something){
  console.log(this.a,something);
  return this.a + something;
}

// 辅助绑定函数
function bind(fn,obj){
  return function(){
    return fn.apply(obj,arguments);
  }
}

var obj = {a:2}
var bar = bind(foo,obj);

var b = bar(3);// 2  3
console.log(b);// 5
```

?> ES5对于硬绑定中提供了内置方法Function.prototype.bind

```js
function foo(something){
  console.log(this.a,something);
  return this.a + something;
}

var obj = {a:2}
var bar = foo.bind(foo);// bind(...)会返回一个硬编码的新函数

var b = bar(3);// 2  3
console.log(b);// 5
```
- ### 2、API调用的“上下文”

?> 第三方库的许多函数，js语言和宿主环境中许多新的内置函数，提供了一种可选的参数，这通常称为“上下文（context）”,作用：确保回调函数指定的this

```js
function foo(el){
  console.log(el,this.id);
}

var obj = {
  id: "nihao"
}

[1,2,3].forEach(foo,obj);

// 1 nihao 2 nihao 3 nihao
```

### 4、new绑定

?> 形式：something = new MyClass(...);

**使用new来调用函数，或发生构造函数调用,会执行以下操作：**

> 1、创建（构造）一个全新的对象

> 2、这个新对象会执行`[[prototype]]`连接

> 3、这个新对象会绑定到函数调用的this

> 4、如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象

```js
function foo(a){
  this.a = a;
}

var bar = new foo(2);// 新对象绑定到foo中的this
console.log(bar.a);// 2
```

## 优先级

?> 上面说到的四种规则（`默认规则`、`显式绑定`、`隐式绑定`、`new绑定`），这些规则的优先级是怎么样的？

**显而，默认规则的优先级是最低的**

- ### 显式绑定 VS 隐式绑定

```js
function foo(){
  console.log(this.a)
}

var obj1 = {
  a: 2,
  foo: foo
}
var obj2 = {
  a: 3,
  foo: foo
}

// 隐式绑定
obj1.foo();// 2
obj2.fooo();// 3

// 显式绑定
obj1.foo.bind(obj2);// 3
obj2.foo.bind(obj);// 2
```

?> 显式绑定优先级更高

- ### new 绑定 VS 隐式绑定

```js
function foo(fn){
  this.a = fn;
}

var obj1 = {
  foo: foo
};

var obj2 = {};

// 隐式绑定
obj1.foo(2);
console.log(obj1.a);// 2

obj1.foo.call(obj2,3);
console.log(obj2.a);// 3

// new 绑定
var bar = new obj1.foo(4);
console.log(obj1.a);// 2
console.log(bar.a);// 4
```

?> new 绑定比隐式绑定优先级高

- ### new 绑定 VS 显式绑定

> new 和call、apply无法一起使用，因此需要硬绑定来测试

```js
function foo(some){
  this.a = some;
}

var obj1 = {};

// 硬绑定
var bar = foo.bind(obj1);
bar(2);

console.log(bar.a);// 2

var baz = new Bar(3);

console.log(obj1.a);// 2
console.log(baz.a);// 3
```

?> new 绑定修改了硬绑定调用的this指向，硬绑定属于显式绑定的一种，因此new绑定优先于显示绑定

- ### 为什么要在new 中使用硬绑定函数呢？

?> 主要的目的是预先设置函数的一些参数，这样在使用new 进行初始化的时候就可以传入其余的参数。主要的功能之一就是把除了第一个参数之外的其他参数传给下层的函数，是`柯里化`的一种

**判断this的规则：**

?> 1、判断是否在 `new` 中调用？如果是，this绑定的是新创建的对象， var bar = new foo();

?> 2、函数是否通过call、apply或者硬绑定（bind）调用？，如果是，this绑定的是指定的对象；var bar = foo.call(obj2);

?> 3、函数是否在某个上下文对象中调用（隐式绑定）？如果是，this绑定的是哪个上下文对象； var bar = obj1.foo();

?> 4、如果都不是的话，则是默认绑定，如果在严格模式下，就绑定到undefined,否则绑定到全局对象； var bar = foo();

## 绑定例外

- ### 被忽略的this

?> 如果把`null`、`undefined`作为this绑定的对象传入call、apply或者bind,这些调用会被忽略，实际应用的`默认绑定规则`

```js
function foo(){
  console.log(this.a);
}

var a = 2;
foo.call( null );// 2  this指向了window
```
比较常见的做法是使用apply或者call、bind进行绑定，apply会传入一个数组（call传入多个参数），bind则对参数进行柯里化

```js
function foo(a,b){
  console.log("结果：" + a + b);
}

// 传入数组
foo.apply(null,[2,3]);

// bind进行柯里化
var bar = foo.bind(null,2);
bar(3);
```

- ### 更安全的this

对于传入null会造成一些副作用，因此可以使用特殊的对象，进行绑定操作

```js
function foo(a,b){
  console.log("结果：" + a + b);
}

var nl = Object.create(null);// 空对象

// 传入数组
foo.apply(nl,[2,3]);

// bind进行柯里化
var bar = foo.bind(null,2);
bar(3);
```

- ### 间接使用

```js
function foo(){
  console.log(this.a);
}

var a = 2;
var o = {a: 3,foo:foo}
var p = {a: 4}

o.foo();// 3
(p.foo=o.foo);// 2
```

?> 对于默认绑定来说，决定this的绑定对象不是调用位置是否处于严格模式，而是函数体是否处于严格模式，如果函数体处于严格模式，this会绑定到undefined,否则this会绑定到全局对象

- ### 软绑定

?> 软绑定不仅可以实现和硬绑定一样的效果，同时保留隐式绑定或者显式绑定修改this的能力

```js
if(!Function.prototype.softBind){
  // 创建原型方法
  Function.prototype.softBind = function(obj){
    var fn = this;
    var curried = [].slice.call(argument,1);// 获取参数
    var bound = function(){
      return fn.apply(
        (!this || this === (window || global))?
        obj:this,
        curried.concat.apply(curried,argument)
      );// 处理this
    };
    bound.prototype = Object.create( fn.prototype );
    return bound;
  }
}
```

## this词法

?> 箭头函数 `=>`,不使用的this的四种标准规则，而是根据外层（函数或者全局）作用域来决定this

```js
function foo(){
  return (a)=>{
    console.log( this.a );
  }
}

var obj1 = {a:2};
var obj2 = {a:3};

var bar = foo.call( obj1 );
bar.call( obj2 );// 2
```

**参考资料：**

[Function.prototype.bind()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)





