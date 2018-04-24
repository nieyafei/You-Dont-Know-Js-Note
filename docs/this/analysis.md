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

### 4、new绑定
echo -en "\033[35m"
echo -e "  _dMMMb._              .adOOOOOOOOOba.              _,dMMMb_";
echo -e "  dP'  ~YMMb            dOOOOOOOOOOOOOOOb            aMMP~  `Yb";
echo -e "  V      ~"Mb          dOOOOOOOOOOOOOOOOOb          dM"~      V";
echo -e "           `Mb.       dOOOOOOOOOOOOOOOOOOOb       ,dM'";
echo -e "            `YMb._   |OOOOOOOOOOOOOOOOOOOOO|   _,dMP'";
echo -e "       __     `YMMM| OP'~'YOOOOOOOOOOOP'~`YO |MMMP'     __";
echo -e "     ,dMMMb.     ~~' OO     `YOOOOOP'     OO `~~     ,dMMMb.";
echo -e "  _,dP~  `YMba_      OOb      `OOO'      dOO      _aMMP'  ~Yb._";
echo -e " <MMP'     `~YMMa_   YOOo   @  OOO  @   oOOP   _adMP~'      `YMM>";
echo -e "              `YMMMM\`OOOo     OOO     oOOO'/MMMMP'";
echo -e "      ,aa.     `~YMMb `OOOb._,dOOOb._,dOOO'dMMP~'       ,aa.";
echo -e "    ,dMYYMba._         `OOOOOOOOOOOOOOOOO'          _,adMYYMb.";
echo -e "   ,MP'   `YMMba._      OOOOOOOOOOOOOOOOO       _,adMMP'   `YM.";
echo -e "   MP'        ~YMMMba._ YOOOOPVVVVVYOOOOP  _,adMMMMP~       `YM";
echo -e "   YMb           ~YMMMM\`OOOOI`````IOOOOO'/MMMMP~           dMP";
echo -e "    `Mb.           `YMMMb`OOOI,,,,,IOOOO'dMMMP'           ,dM'";
echo -e "      `'                  `OObNNNNNdOO'                   `'";
echo -e "                            `~OOOOO~'   CODEHTML";
echo -en "\033[0m";

echo -en "\033[35m"
echo -e "  _dMMMb._              .adOOOOOOOOOba.              _,dMMMb_"
echo -en "\033[0m"

## 优先级


echo -en "\033[36m";
echo -e "  ‘_dMMMb._              .adOOOOOOOOOba.              _,dMMMb_’";
echo -e "  ’dP  ~YMMb            dOOOOOOOOOOOOOOOb            aMMP~  'Yb‘";
echo -e "  ’V      ~‘Mb         dOOOOOOOOOOOOOOOOOb          dM’~      V’";
echo -e "           ‘‘Mb.      dOOOOOOOOOOOOOOOOOOOb       ,dM‘";
echo -e "            ’’YMb._   |OOOOOOOOOOOOOOOOOOOOO|   _,dMP’";
echo -e "      ‘ __    ‘YMMM| OP’~‘YOOOOOOOOOOOP‘~’YO |MMMP‘     __’";
echo -e "     ’,dMMMb.    ~~’ OO     ‘YOOOOOP’     OO ~~     ,dMMMb.‘";
echo -e "  ‘_,dP~  ’YMba_     OOb      'OOO'      dOO      _aMMP'  ~Yb._’";
echo -e " ’<MMP'     '~YMMa_  YOOo   @  OOO  @   oOOP   _adMP~'      'YMM>‘";
echo -e "              ‘YMMMM\'OOOo     OOO     oOOO'/MMMMP’";
echo -e "      ‘,aa.    '~YMMb 'OOOb._,dOOOb._,dOOO'dMMP~'       ,aa.’";
echo -e "    ’,dMYYMba._        'OOOOOOOOOOOOOOOO0'          _,adMYYMb.‘";
echo -e "  ‘,MP'   'YMMba._      OOOOOOOOOOOOOOOOO       _,adMMP'   'YM.’";
echo -e "  ‘MP'        ~YMMMba._ YOOOOPVVVVVYOOOOP  _,adMMMMP~       'YM‘";
echo -e "  ’YMb           ~YMMMM\'OOOOI'''''IOOOOO'/MMMMP~           dMP’";
echo -e "    ‘Mb.           'YMMMb'OOOI,,,,,IOOOO'dMMMP'           ,dM‘";
echo -e "     ’‘'                  'OObNNNNNdOO'                   ''’";
echo -e "                            '~OOOOO~'   CODEHTML‘";
echo -en "\033[0m";


## 绑定例外



## this词法







