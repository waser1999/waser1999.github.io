---
title: Vue.js语法解析
date: 2024-04-12 19:24:51
tags:
- vue
- CORS
- 展开语法符
---
今天碰到了一些问题，在此总结如下。
<!--more-->
## 1. 导入组件的两种方法的区别
### a. `import`方法导入 
是ES6标准（js语言标准）中新增的导入方法。是静态执行（编译时执行）的。  
i. `import Aa from'./A'`，表示导入A模块下的所有处于`export default`下的变量与方法。此时`Aa`可以随意命名。  
ii. `import { Aa } from'./A'`表示导入A模块下可`export`的所有变量中命名为`Aa`的变量。此时，`Aa`不能随意命名。  
`import`方法导出需要使用`export default` 或类似语句。  
### b. `require`方法导入
node.js中存在的导入方法。语法：`const myModule = require('./myModule')`。是动态执行的。  
`require`使用`module.export = {}`方法导出。  
## 2. CORS（跨域）问题
a. 跨域的标志就是浏览器控制台出现如下报错： 
```
Access to XMLHttpRequest at 'https://xxx.xxx.com' from origin 'https://xxx.xxx.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
b. 哪些情况下会产生跨域？
```
URL                             说明                是否允许通信
http://www.a.com/a.js
http://www.a.com/b.js           同一域名下            允许

http://www.a.com/lab/a.js
http://www.a.com/script/b.js    同一域名下不同文件夹    允许

http://www.a.com:8000/a.js
http://www.a.com/b.js           同一域名，不同端口      不允许

http://www.a.com/a.js
https://www.a.com/b.js          同一域名，不同协议      不允许

http://www.a.com/a.js
http://70.32.92.74/b.js         域名和域名对应ip        不允许

http://www.a.com/a.js
http://script.a.com/b.js        主域相同，子域不同      不允许（cookie这种情况下也不允许访问）

http://www.a.com/a.js
http://a.com/b.js               同一域名，不同二级域名（同上） 不允许（cookie这种情况下也不允许访问）

http://www.cnblogs.com/a.js
http://www.a.com/b.js           不同域名            不允许
```
> 简而言之，即同一域名不同端口、同一域名不同网络协议、不同子域名、不同域名之间不能互相通信。**浏览器限制从脚本发起的跨域http请求。**  

c. node.js可以通过加入`cors`模块，并在`route`下使用`route.use(cors())`配置即可。以下是一个配置所有网站均可跨域的例子：  
```
router.use(cors({
  origin: "*",
}));
```
> 实践中并不推荐将所有网站设为均可跨域访问，此处仅作演示。  
    
## 3. ref和reactive两者的区别
a. `reactive`只能用于存储对象，不能用于存储基本数据类型。而`ref`两者皆可。  
例如你可以声明：
```
const count = ref(0);
const count = ref({count : 0});
```
却只能声明：
```
const state = reactive({ count: 0 });
```
b. `ref`可以直接赋值数据，但`reactive`不能直接赋值数据本身（因为是对象），只能赋值数据的一部分属性。例如:
```
let objData = reactive({
    name:'林漾',
    age:31,
    sex:'女'
})
        
function submitHander(){
    objData = {
        name:'林漾1',
        age:31,
        sex:'女nv'	
    }
}
```
会无法更新objData对象，因为直接将新的json数据赋给了reactive对象。要正确赋值，应如此做：
```
function submitHander(){
    objData.name='林漾1';
    objData.age='31';
    objData.sex='女nv';
}
```

c. `ref`数据属于引用类型，因此在js脚本下需要额外使用`value`属性访问。但在`<template>`和`css`中提供了简化方法，无需`value`数据。例如，在js端访问的代码需如此做：
```
const count = ref(0)

function increment() {
  count.value++
}
```
而在`<template>`中只需如此做：
```
<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```
## 4. 展开运算符（`...`）
展开运算符可以将一组对象一个一个分别展开，相当于循环遍历子对象。例如：
```
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const mergedArray = [...arr1, ...arr2]; // 将两个数组合并成一个新数组
console.log(mergedArray); 
// 输出: [1, 2, 3, 4, 5, 6]
```