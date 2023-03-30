# 语言基础

## 变量

### var

不初始化的情况下，变量会保存一个特殊值undefined。

==作用域==——使用var 操作符定义的变量会成为包含它的函数的局部变量。比如，使用var在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁：

```javascript
function test(){
    var message = 'hi';
}
test();
console.log(message)//出错
```

==声明提升==

var关键字声明的变量会自动提升到**函数作用域顶部**：

```javascript
function foo() {
console.log(age);
var age = 26;
}
foo(); // undefined
//=====================
function foo() {
var age;
console.log(age);
age = 26;
}
foo(); // undefined
```



### let

==作用域==——块作用域

```javascript
if (true) {
var name = 'Matt';
console.log(name); // Matt
}
console.log(name); // Matt
//=============================
if (true) {
let age = 26;
console.log(age); // 26
}
console.log(age); // ReferenceError: age 没有定义
```

**块作用域是函数作用域的子集**——let范围更小

==全局声明==

与var 关键字不同，使用let 在全局作用域中声明的变量不会成为window 对象的属性（var 声明的变量则会）。

```javascript
var name = 'Matt';
console.log(window.name); // 'Matt'
let age = 26;
console.log(window.age); // undefined
```

==for 循环中的let 声明==

let声明的迭代变量的作用域仅限于for 循环块内部，var声明的迭代变量会渗透到循环体外部。



### const

声明变量时必须同时初始化变量，且尝试修改const 声明的变量会导致运行时错误。

==作用域也是块==

```javascript
const name = 'Matt';
if (true) {
const name = 'Nicholas';
}
console.log(name); // Matt
```

const 声明的限制只适用于它指向的变量的引用，如果const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反const 的限制。

```javascript
const person = {};
person.name = 'Matt'; // ok
```

不适用for-i循环，但是对for-of 和for-in 循环特别有意义

```javascript
let i = 0;
for (const j = 7; i < 5; ++i) {
console.log(j);
}
// 7, 7, 7, 7, 7
for (const key in {a: 1, b: 2}) {
console.log(key);
}
// a, b
for (const value of [1,2,3,4,5]) {
console.log(value);
}
// 1, 2, 3, 4, 5
```

### 声明风格及最佳实践

1. 不使用var

2. const优先，let次之

   只在提前知道未来会有修改时，再使用let。

   

## 数据类型

ECMAScript 有6 种简单数据类型（也称为==原始类型==）：Undefined、Null、Boolean、Number、String 和Symbol。

还有一种==复杂数据类型==叫Object（对象）。Object 是一种无序名值对的集合。

### typeof 操作符

*  "undefined"表示值未定义；
*  "boolean"表示值为布尔值；
*  "string"表示值为字符串；
*  "number"表示值为数值；
*  "object"表示值为对象（而不是函数）或null；
*  "function"表示值为函数；
*  "symbol"表示值为符号。

typeof ==是一个操作符而不是函数==

```javascript
let message = "some string";
console.log(typeof message); // "string"
console.log(typeof(message)); // "string"
console.log(typeof 95); // "number"
```

调用typeofnull 返回的是"object"。这是因为特殊值null 被认为是一个对空对象的引用。



### Undefined 类型

当使用var 或let 声明了变量但没有初始化时，就相当于给变量赋予了undefined 值：

```javascript
let message;
console.log(message == undefined); // true
```

它和undefined 的字面值时，两者是相等的。这个例子等同于如下示例：

```javascript
let message = undefined;
console.log(message == undefined); // true
```

==一般来说，永远不用显式地给某个变量设置undefined 值。字面值undefined主要用于比较，增加这个特殊值的目的就是为了正式明确空对象指针（null）和未初始化变量的区别。==

undefined 值的变量跟未定义变量是有区别的：

```javascript
let message; // 这个变量被声明了，只是值为undefined
// 确保没有声明过这个变量
// let age
console.log(message); // "undefined"
console.log(age); // 报错
```

在对未初始化的变量调用typeof 时，返回的结果是"undefined"，但对未声明的变量调用它时，返回的结果还是"undefined":

```javascript
let message; // 这个变量被声明了，只是值为undefined
// 确保没有声明过这个变量
// let age
console.log(typeof message); // "undefined"
console.log(typeof age); // "undefined"
/*建议在声明变量的同时进行初始化。
这样，当typeof 返回"undefined"时，
你就会知道那是因为给定的变量尚未声明，而不是声明了但未初始化。*/
```

==undefined 是一个假值==

```javascript
let message; // 这个变量被声明了，只是值为undefined
// age 没有声明
if (message) {
// 这个块不会执行
}
if (!message) {
// 这个块会执行
}
if (age) {
// 这里会报错
}
```



### Null类型

逻辑上讲，null 值表示一个空对象指针。在定义将来要保存对象值的变量时，建议使用null 来初始化，不要使用其他值。这样，只要检查这个变量的值是不是null 就可以知道这个变量是否在后来被重新赋予了一个对象的引用。

==undefined 值是由null 值派生而来：==

```javascript
console.log(null == undefined); // true
```

==注意：====操作符会为了比较而转换它的操作数。

==null 是一个假值。==



### Boolean 类型

两个字面值：true 和false，区分大小写的。

要将一个其他类型的值转换为布尔值，可以调用特定的Boolean()转型函数：

```javascript
let message = "Hello world!";
let messageAsBoolean = Boolean(message);
```

Boolean()转型函数可以在任意类型的数据上调用，而且始终返回一个布尔值。什么值能转换为true或false 的规则取决于数据类型和实际的值:

| 数据类型  | 转换为true 的值        | 转换为false 的值 |
| --------- | ---------------------- | ---------------- |
| Boolean   | true                   | false            |
| String    | 非空字符串             | ""（空字符串）   |
| Number    | 非零数值（包括无穷值） | 0、NaN           |
| Object    | 任意对象               | null             |
| Undefined | N/A（不存在）          | undefined        |

像if 等流控制语句会==自动执行==其他类型值到布尔值的==转换==：

```javascript
let message = "Hello world!";
if (message) {
console.log("Value is true");
}
```



### Number 类型

```javascript
let intNum = 55; // 十进制整数
let octalNum1 = 070; // 八进制的56
let octalNum2 = 079; // 无效的八进制值，当成79 处理
let octalNum3 = 08; // 无效的八进制值，当成8 处理
let hexNum1 = 0xA; // 十六进制10
let hexNum2 = 0x1f; // 十六进制31
```

### 浮点值

要定义浮点值，数值中必须包含小数点，而且小数点后面必须至少有一个数字。

```javascript
let floatNum1 = 1.1;
let floatNum2 = 0.1;
let floatNum3 = .1; // 有效，但不推荐
let floatNum1 = 1.; // 小数点后面没有数字，当成整数1 处理
let floatNum2 = 10.0; // 小数点后面是零，当成整数10 处理
let floatNum = 3.125e7; // 等于31250000
```

浮点值的精确度最高可达==17 位小数==，但在算术计算中远不如整数精确。例如，0.1 加0.2 得到的不是0.3，而是0.300 000 000 000 000 04。由于这种微小的舍入错误，导致很难测试特定的浮点值。比如下面的例子：



### 值的范围

ECMAScript 可以表示的最小数值保存在Number.MIN_VALUE 中，这个值在多数浏览器中是5e324；可以表示的最大数值保存在Number.MAX_VALUE 中，这个值在多数浏览器中是1.797 693 134 862 315 7e+308。如果某个计算得到的
数值结果超出了JavaScript 可以表示的范围，那么这个数值会被自动转换为一个特殊的Infinity（无穷）值。任何无法表示的负数以-Infinity（负无穷大）表示，任何无法表示的正数以Infinity（正无穷大）表示。

要确定一个值是不是有限大（即介于JavaScript 能表示的最小值和最大值之间），可以使用isFinite()函数。

```javascript
let result = Number.MAX_VALUE + Number.MAX_VALUE;
console.log(isFinite(result)); // false
```



### NaN

有一个特殊的数值叫NaN，意思是“不是数值”（Not a Number），用于表示本来要返回数值的操作失败了（而不是抛出错误）。比如，用0 除任意数值在其他语言中通常都会导致错误，从而中止代码执行。但在ECMAScript 中，0、+0 或0 相除会返回NaN



### 数值转换

有3 个函数可以将非数值转换为数值：Number()、parseInt()和parseFloat()。Number()是转型函数，可用于任何数据类型。后两个函数主要用于将字符串转换为数值。对于同样的参数，这3 个函数执行的操作也不同。









































