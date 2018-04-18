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


### 2、隐式绑定


### 3、显式绑定


### 4、new绑定



## 优先级



## 绑定例外



## this词法







