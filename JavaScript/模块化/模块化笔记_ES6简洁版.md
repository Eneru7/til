# 一、模块化简介

## 1、背景

创建04-module-demo目录，创建 “no-module” 目录，创建a.js 

```javascript
var star = '周杰伦'
```

创建b.js 

```javascript
var star = 5
```

创建demo.html

从这个例子可以看出，star的值的不确定性很大，a和b两个脚本文件中的同名变量互相干扰。 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script src="a.js"></script>
  <script src="b.js"></script>
  <script>
    console.log(star)
  </script>
</body>
</html>
```

## 2、模块化解决的问题

模块化主要解决javascript程序全局空间内被污染的问题

## 3、模块化规范

- CommonJS模块化规范（基于ES6语法之前）

- - 例如：node.js中的 require 引入模块，exports 导出模块

- ES6模块化规范（使用ES6语法）

- - export 导出模块
  - import 引入模块

# 二、ES6模块化规范



## 1、导出模块

创建"module"目录，创建m1.js 

```javascript
export let star = '周杰伦';
export function sing(){
  console.log('花海')
}
```

还可以这样导出 

```javascript
let star = '周杰伦';
function sing(){
  console.log('花海')
}
export {star, sing}
```

创建m2.js 

```javascript
export let star = 5
```

2、导入模块

创建demo.html 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script type="module">
    import * as m1 from './m1.js'
    import * as m2 from './m2.js'
    console.log(m1)
    console.log(m2)
    console.log(m1.star)
    console.log(m2.star)
    
    //还可以这样导入：解构赋值的形式
    import {star, sing} from './m1.js'
    import {star as star2} from './m2.js' //使用as防止重名
    console.log(star)
    sing()
    console.log(star2)
    
  </script>
</body>
</html>
```


3、测试运行html文件测试
三、默认暴露模块1、默认暴露创建m3.js 

```javascript
//可以导出任意类型，但同时只能有一个export default
// export default 'helen'
// export default 99
//大部分情况是导出对象
export default{
  username: 'helen',
  age: 20,
  coding(){
    console.log('hello world')
  } 
}
```

2、导入模块在demo.html中导入模块 

```javascript
//导入m3
import m3 from './m3.js'
console.log(m3)
```

3、测试运行html文件测试
四、封装代码1、创建app.js可以看做是程序的入口文件 

```javascript
import * as m1 from './m1.js'
import * as m2 from './m2.js'
import m3 from './m3.js'
console.log(m1)
console.log(m2)
console.log(m3)
```

2、引入入口文件创建demo-app.html 

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
  <script src="app.js" type="module"></script>
</body>
</html>
```

## 3、测试

运行html文件测试