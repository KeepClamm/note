`1.vue3的watch的使用`

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

`2.ref和reactive`

