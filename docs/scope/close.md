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

**修改一下上面的代码：**

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

?> `bar`函数本身被当做一个值类型进行传递, `foo()`执行后，返回值赋值给变量`baz`,变量baz调用，其实就是通过不同的标识符调用了内部函数`bar()`

**继续看几个闭包的代码块：**

```js
function foo(){
  var a = 2;

  function baz(){
    console.log(a);
  }

  bar(baz)
}

function bar(fn){
  fn();// 闭包
}
```
**间接传递函数也是ok的**
```js
var fn;

function foo(){
  var a = 2;

  function baz(){
    console.log( a );
  }

  fn = baz;// 将baz分配给全局变量
}

function bar(){
  fn();
}

foo();// 赋值fn = baz

bar();// 2
```

### 你懂了吗？

```js
function wait(message){
  setTimeout(function(){
    console.log(message)
  },1000)
}

wait("CODEHTML");
```

> 此代码块，参数传入定时器内，1秒之后执行，他的内部作用域不会消失，定时器的函数依然保有`wait(...)`作用域的闭包

**通常大家认为`IIFE模式`是经典的闭包模式，但是这本书的作者不太认同**

```js
var a = 2;

(function IIFE(){
  console.log(a);
})()
```




