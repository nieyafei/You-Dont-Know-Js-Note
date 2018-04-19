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

### 3、显式绑定


### 4、new绑定



## 优先级



## 绑定例外



## this词法







