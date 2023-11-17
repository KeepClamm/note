`vue3的watch的使用`

1.watch监听器监听一个getter函数

```javascript
import { reactive, watch } from 'vue'
const state = reactive({ count: 0 })
watch(() => state.count, (newValue, oldValue) => {
    // 因为watch被观察的对象只能是getter/effect函数、ref、active对象或者这些类型是数组
    // 所以需要将state.count变成getter函数
})
```

2.watch可以监听响应式对象

```javascript
import { ref, watch } from 'vue' 
const count = ref(0) 
watch(count, (newValue, oldValue) => {
})
```

3.watch可以监听多个响应式对象，任何一个响应式对象更新，就会执行回调函数。

```javascript
import { ref, watch } from 'vue' 
const count = ref(0) 
const count2 = ref(1) 
//第一种写法
watch([count, count2], ([newCount, newCount2], [oldCount, oldCount2]) => { 
})
//还有第二种写法
watch([count, count2], (newValue, oldVlaue) => {
    console.log(newValue)//[newCount, newCount2]
    console.log(oldValue)//[oldCount, oldCount2]
})
```

`toRef`

toRef可以根据一个响应式对象中的一个属性，创建一个响应式的ref。同时这个ref和原对象中的属性保持同步，改变原对象属性的值这个ref会跟着改变，反之改变这个ref的值原对象属性值也会改变。它接收两个参数，一个是响应式对应，另一个则是属性值。

`toRefs`

toRefs可以将一个响应式对象转化成普通对象，而这个普通对象的每个属性都是响应式的ref

`reactive`

| reactive                         | ref                                                    |
| -------------------------------- | ------------------------------------------------------ |
| 只支持对象和数组（引用数据类型） | 支持基本数据类型+引用数据类型                          |
| 在<script>和<template>无差别使用 | 在<script>和<template>使用方式不同（script中要.value） |
| 重新分配一个新对象会丢失响应性   | 重新分配一个新对象不会丢失响应                         |
| 将对象传入函数时，失去响应       | 传入函数时，不会失去响应                               |

