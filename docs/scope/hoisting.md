# 提升

## 先有鸡还是先有蛋

**考虑以下代码,会输出什么结果？**

```js
a = 2;
var a;
console.log(a);
```

?> 此题的答案`2`, 不要以为`var a`在`a = 2`后，输出的结果是`undefined`, 其实代码可以理解如下执行：

```js
var a;// 变量申明被提升
a = 2;// 赋值
console.log(a);
```

**再看一段代码：**

```js
console.log(a);
var a = 2;
```

?> 此题结果`undefined`, 因为变量会被提升，代码执行如下：

```js
var a;
console.log(a);
a = 2;
```

具体详细的的解释，先有蛋（声明），还是先有鸡（赋值），下面通过编译器解释

## 编译器再度来袭

?> 包括变量和函数在内的所有声明都会在任何代码被执行前首先被处理

下面来解析上面的题目吧：

> **1、在编译阶段执行的是`定义声明`,而赋值阶段则在原地等待执行阶段**

> **2、因此`var a = 2`会被解析成，先声明变量 `var a`;(默认赋值`undefined`)**

> **3、接着执行到赋值阶段 `a = 2;`，这个时候的a被赋值了2**

?> 这就是所谓的`变量提升`，换句话说：先有声明（蛋）后有赋值（鸡）

**再来看一段代码：**

```js
foo();

function foo(){
  console.log(a);
  var a = 2;
}
```

> 通过上面的解释，内部的打印结果，很显然是默认值`undefined`

?> 注意一下这个代码块，`foo()`的执行时在函数声明之前, 这也就是说`foo(){}`函数的声明也被`提升`了

**注意：**

?> 每个作用域都会进行提升操作，虽然函数声明也会被提升，但是**函数表达式却不会被提升**

```js
foo();// TypeError!
var foo = function(){
  // something
}
```

> foo被声明提升，但是默认值是`undefined`, 执行到`foo()`的调用，则会报`TypeError(类型错误)`

如果代码修改一下，结果会是怎么样的？

```js
foo();// TypeError!
bar();// ReferenceError
var foo = function bar(){
  // something
}
```

?> 因为具名的函数表达式，名称标识符在赋值前无法在所在的作用域中使用的，因此上个代码块，就会报错`ReferenceError（引用错误）`

## 函数优先

?> 从上面可以知道，**函数声明和变量声明都会被提升，但是函数声明会先被提升，然后才是变量**

下面通过代码来看一下：

```js

foo();

var foo;

function foo(){
  console.log(1);
};

foo = function(){
  console.log(2);
}
```

?> 此题的答案是 `1`

> **函数声明会被提升到普通变量之前**

?> **重复的声明变量会被忽略，但是后面的函数声明会覆盖掉前面的：如下代码：**

```js
foo();

function foo(){
  console.log(1);
}

var foo = function(){
  console.log(2);
}

function foo(){// 会覆盖上面的
  console.log(3);
}
```

?> 此题打印`3`

**如果在最后再执行一次`foo()`, 那么结果如何?**

?> 打印`2`, 因为对`foo`被重新赋值声明

**下面看一个代码块？**

```js
foo();
var flag = true;
if(flag){
  function foo(){
    console.log("a");
  }
}else{
  function foo(){
    console.log("b");
  }
}
```
?> 打印结果是`b`,**不建议这种写法，避免在块内部声明函数**