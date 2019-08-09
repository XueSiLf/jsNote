# 标准对象

[TOC]

## 标准对象简介

在JavaScript的世界里，一切都是对象。

但是某些对象还是和其他对象不太一样。为了区分对象的类型，我们用`typeof`操作符获取对象的类型，它总是返回一个字符串：

~~~javascript
typeof 123;// 'number'
typeof NaN;// 'number'
typeof 'str';// 'string'
typeof true;// 'boolean'
typeof undefined;// 'undefined'
typeof Math.abs;// 'function'
typeof null;// 'object'
typeof [];// 'object'
typeof {};// 'object'
~~~

可见，`number`、`string`、`boolean`、`function`和`undefined`有别于其他类型。特别注意`null`的类型是`object`，`Array`的类型也是`object`，如果我们用`typeof`将无法区分出`null`、`Array`和通常意义上的object——`{}`。

### 包装对象

#### 【留心：什么叫包装对象？Number Boolean String】

除了这些类型外，JavaScript还提供了包装对象，熟悉Java的小伙伴肯定很清楚`int`和`Integer`这种暧昧关系。

`number`、`boolean`和`string`都有包装对象。没错，在JavaScript中，字符串也区分`string`类型和它的包装类型。包装对象用`new`创建：

~~~javascript
var n = new Number(123);// 123,生成了新的包装类型
var b = new Boolean(true);// true,生成了新的包装类型
var s = new String('str');// 'str',生成了新的包装类型
~~~

#### 【留心：注意 包装对象 和 原始值 的 区别】

虽然包装对象看上去和原来的值一模一样，显示出来也是一模一样，但他们的类型已经变为`object`了！所以，包装对象和原始值用`===`比较会返回`false`：

~~~javascript
typeof new Number(123);// 'object'
new Number(123) === 123;// false
typeof new Boolean(true);// 'object'
new Boolean(true) === true;// false
typeof new String('str');// 'object'
new String('str') === 'str';// false
~~~

#### 【所以尽量不要用包装对象，切记。】尤其是string类型！

如果我们在使用`Number`、`Boolean`和`String`时，没有写`new`会发生什么情况？

#### 【留心：强制类型转换】

此时，`Number()`、`Boolean`和`String()`被当做普通函数，把任何类型的数据转换为`number`、`boolean`和`string`类型（注意不是其包装类型）：

~~~javascript
var n = Number('123');// 123，相当于parseInt() 或 parseFloat()
typeof n;// 'number'

var b = Boolean('true');// true
typeof b;// 'boolean'

var b2 = Boolean('fasle');// true! 'false'字符串转换结果为true！因为它是非空字符串！
var b3 = Boolean('');// false

var s = String(123.45);// '123.45'
typeof s;// 'string'
~~~

### 总结一下：

- 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
- 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
- 用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
- 通常不必把任意类型转换为`boolean`再判断，因为可以直接写`if (myVar) {...}`；
- `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`；
- 判断`Array`要使用`Array.isArray(arr)`；
- 判断`null`请使用`myVar === null`；
- 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
- 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。

#### 【留心：不是任何对象都有toString()方法，null 和 undefined 就没有】

最后有细心的同学指出，任何对象都有`toString()`方法吗？`null`和`undefined`就没有！确实如此，这两个特殊值要除外，虽然`null`还伪装成了`object`类型。

#### 【留心：number对象调用 toString() 方法报错】

更细心的同学指出，`number`对象调用`toString()`报SyntaxError：

~~~JavaScript
123.toString();// SyntaxError语法错误
~~~

遇到这种情况，要特殊处理一下：

~~~JavaScript
123..toString();// '123'，注意是两个点
(123).toString();// '123'
~~~





## Date 用来表示日期和时间

在JavaScript中，`Date`对象用来表示日期和时间。

### 【留心：如何获取系统当前时间】

~~~javascript
var now = new Date();
now;// Sat Aug 10 2019 01:31:15 GMT+0800 (中国标准时间)
now.getFullYear();// 2019，年份
now.getMonth;// 7，月份，注意月份范围是0~11，7表示八月
now.getDate();// 10，表示10号
now.getDay();// 6，表示星期六
now.getHours();// 01，24小时制
now.getMinutes();// 31，分钟
now.getSeconds();// 15，秒
now.getMilliseconds();// 888，毫秒数
now.getTime();// 1111111111，以number形式表示的时间戳
~~~

### 【留心：当前时间不一定准确，来自本地电脑】

注意，当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。

如果要创建一个指定日期和时间的`Date`对象，可以用：

~~~JavaScript
var d = new Date(2019, 7, 10, 9, 30, 00, 123);
d;//
~~~

### 【留心：月份用0-11来表示 1-12月】

有个非常坑的地方，就是JavaScript的月份范围用整数表示是0~11，`0`表示一月，`1`表示二月……，所以要表示8月，我们传入的是`7`！这绝对是JavaScript的设计者当时脑抽了一下，但是现在要修复已经不可能了。

#### 【注意：JavaScript的Date对象月份值从0开始，牢记0=1月，1=2月，2=3月，……，11=12月。】

第二种创建一个指定日期的 和 时间的方法是解析一个符合[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式的字符串：

比如：

~~~JavaScript
var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d;// 1435146562875，返回的是一个时间戳
~~~

但它返回的不是`Date`对象，而是一个时间戳。不过有时间戳就可以很容易地把它转换为一个`Date`：

### 【留心：时间戳转成具体日期时间】

~~~javascript
var d = new Date(1435146562875);
d;// Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
~~~

#### 【留心：使用Date.parse()时传入的字符串使用实际月份01~12，转换为Date对象后getMonth()获取的月份值为0~11。】



### 时区：

`Date`对象表示的时间总是按浏览器所在时区显示的，不过我们既可以显示本地时间，也可以显示调整后的UTC时间：