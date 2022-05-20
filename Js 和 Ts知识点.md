# undefined 和 null 的区别

1. `undefined` 不是关键字，而 `null`是关键字

   将某个变量赋值为undefined 或者null，实际上没有太大差别，都表示这个变量的值为“空”

   但是有如下情况：

   ```javascript
   var undefined = "";
   var null = "";//Unexpected token 'null'
   ```

2. 二者被转换为布尔值时，都是false

   ```javascript
   console.log(new Boolean(undefined))//Boolean{false}
   console.log(new Boolean(null))//Boolean{false}
   ```

3. `undefined`在和`null`进行==比较时两者相等，全等于比较时两者不等

   ```javascript
   console.log(undefined == null)//true
   console.log(undefined === null)//false
   ```

4. 使用Number()对undefined和null进行[类型转换](https://so.csdn.net/so/search?q=类型转换&spm=1001.2101.3001.7020)

   ```javascript
   console.log(Number(undefined))//NaN
   console.log(Number(null))//0
   ```

5. undefined本质上是window的一个属性，而null是一个对象

6. undefined和null的用途 

   * null表示没有对象，即不应该有值，经常用作函数的参数，或作为原型链的重点。

   * undefined表示缺少值，即应该有值，但是还没有赋予（变量提升时默认会赋值为undefined，函数参数未提供默认为undefined，函数的返回值默认为undefined）

     





# TypeScript中的`?:` 、`??`、`?.`、`!.`

* `?:`：指可选参数，可以理解为参数自动加上`undefined`

* `??`：和`||`的意思相似，但区别如下

  ```typescript
  console.log(null || 3)//3
  console.log(null ?? 2)//2
  
  console.log(undefined || 2)//2
  console.log(undefined ?? 2)//2
  
  console.log(0 || 2)//2
  console.log(0 ?? 2)//2
  ```

* `?.`：和`&&`相似 `a ?. b`相当于`a && a.b ? a.b : undefined`

  ```typescript
  const a = {
      b:{
          c:7
      }
  }
  console.log(a?.b?.c)//7
  console.log(a && a.b && a.b.c)//7
  ```

