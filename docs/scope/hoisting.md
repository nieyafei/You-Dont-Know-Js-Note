# 提升

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


## 先有鸡还是先有蛋

## 编译器再度来袭

## 函数优先


