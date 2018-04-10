# 实质问题到懂闭包

?> 当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行

**先看一个简单的闭包：**

```js
function foo(){
  var a = 2;
  function bar(){
    copnsole.log(2);
  }
  bar();
}
foo();// 2
```

> 这块代码是一个`RHS`引用查询，`bar()`嵌套在`foo()`的内部，封闭在`foo()`的作用域中

***修改一下上面的代码：*

```js
function foo(){
  var a = 2;
  function bar(){
    copnsole.log(2);
  }
  return bar;
}
var baz = foo();
baz();
```



