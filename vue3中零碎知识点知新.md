新建vite项目

1. npm init vite@latest
2. (project name)

如果选择ts版本，新建的vite项目采用了setup语法糖写法，如：

```vue
<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + TypeScript + Vite" />
</template>
```



v-bind 

作用：本质是将js中的数据传入template中，单向的

* 带参数：该参数就是标签的一个属性，用一个变量进行动态赋值

* 不带参数：就是直接传入一个对象所有的property

  ```vue
  // 对象
  post: {
    id: 1,
    title: 'My Journey with Vue'
  }
  // 模板
  <blog-post v-bind="post"></blog-post>
  // 等价于	
  <blog-post v-bind:id="post.id" v-bind:title="post.title"></blog-post>
  
  
  ```

  

v-model 

作用：双向绑定，这是一个语法糖。本质上是v-bind一直属性，然后监听这个属性的变化并处理。



传入 ref 指什么