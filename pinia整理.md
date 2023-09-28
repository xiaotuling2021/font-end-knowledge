# pinia函数式编程整理

1.pinia的优点

- 所见即所得
- 类型安全
- 开发工具支持
- 可扩展性
- 模块化设计
- 极致轻量化

2.安装

```
npm i pinia
```

3.创建根store，并传递给应用

```
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()

createApp(App).use(pinia).mount('#app')
```

4.定义store

```
import { defineStore } from 'pinia'
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  function increment() {
    count.value++
  }
  return {count, increment}
});
```

ref：数据

computed：计算属性

function：方法

5.通过storeToRefs将store中的数据和计算属性变成响应式

```
import {useCounterStore} from './counter'
import { storeToRefs } from 'pinia';
const store = useCounterStore()
const { name } = storeToRefs(store)
const { increment } = store
```

6.重置

```
store.$reset() // 可以将store里面的state重置为初始值
```

7.变更state

```
store.$reset((state) => {
  state.item.push({
    name: 'shoes',
    quantity: 1
  })
  state.hasChanged = true
})
```

8.替换state

```
store.$patch({count: 24})
```

