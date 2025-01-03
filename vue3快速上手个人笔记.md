# vue3 快速上手个人笔记

## 1.创建脚手架

### **1.1 终端输入**

```bash
npm creat vue@latest
```

**首次安装需要 `create-vue`**

![image-20241218212352177](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241218212352177.png)

`下面的配置跟着选`



> 💡
> 注意 ⚠️ 这里项目名不能中文

![image-20241218212405173](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241218212405173.png)

### 1.2 安装依赖

> 这里要进到项目 文件安装依赖
>

```bash
npm i
```

> 不然下面的文件会报错
>

![image-20241218212442578](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241218212442578.png)

## 2.了解 main.ts

```bash
import { createApp } from 'vue'
```

`Vue3` **中是通过** `createApp` **函数创建一个应用实例**

------

```bash
import App from './App.vue'
```

`App` **是分支**

------

```bash
createApp(App)
```

**`createApp**(App)` ** 是创建一个应用的根组件在 `App` 里**

------

```bash
.mount('#app')
```

**`.mount('#app')`** **挂载在 `app` 的文件里**

------

### 1.1 这里必须要的两个 `src` 的文件

![image-20241218212537875](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241218212537875.png)

## 3.`App.vue` 里的 `setup`

```html
<script setup lang="ts">
const message: string = 'Hello from TypeScript!';
</script>
```

**为什么不写 `setup` 会报错？**

当你不使用 `setup` 并且不导出 `setup()` 函数时，Vue 编译器不知道如何处理 `<script>` 标签内的代码。它不知道哪些变量应该暴露给模板，也不知道如何处理响应式数据。因此，它会报错。

具体来说，它可能会报以下类型的错误：

- **模板中找不到变量：** 如果你在模板中使用了某个变量，而该变量没有在 `setup()` 函数中 `return`，Vue 会提示该变量未定义。
- **类型错误：** 如果你使用了 TypeScript，并且没有正确地定义类型，TypeScript 编译器会报错

### 3.1 `css` 样式

```css
background-color: #ddd;
box-shadow: 0 0 10px;
border-radius: 10px;
padding: 20px;
```

- **`background-color: #ddd;`**: 设置背景颜色为浅灰色（`#ddd` 是十六进制颜色码）。
- **`box-shadow: 0 0 10px;`**: 添加阴影。`0 0 10px` 分别表示水平偏移、垂直偏移和模糊半径。`0 0` 表示阴影不偏移，`10px` 表示阴影的模糊程度。
- **`border-radius: 10px;`**: 设置边框圆角，半径为 10 像素。
- **`padding: 20px;`**: 设置内边距，即内容与边框之间的距离，上下左右均为 20 像素。

## 4.面试问题

> 💡 `setup` 与 `Options API` 的关系

**核心要点：**

`setup` 可以和 `data`、`methods` 同时存在于一个 Vue 组件中，但这种混合使用方式存在严重的问题，主要是不一致性和潜在的错误。

**详细说明：**

1. **`data` 和 `methods` 可以访问 `setup` 中的值：** 这是 **单向的访问**。在 `data` 和 `methods` 中，你可以通过 `this` 访问到 `setup` 中返回的任何属性和方法。
2. **`setup` 理论上不能使用 `this`：** `setup` 函数是在组件实例创建 *之前* 执行的。这意味着在 `setup` 函数执行时，组件实例（也就是 `this`）还没有被创建。因此，在 `setup` 内部使用 `this` 会得到 `undefined`。
3. **`setup` 不能访问 `data` 和 `methods`：** 由于 `setup` 执行时组件实例尚未创建，所以它无法通过 `this` 访问 `data` 和 `methods` 中定义的内容。如果尝试在 `setup` 中使用 `this` 来访问 `data` 或 `methods`，会导致运行时错误。

## 5.如果文件名非要呈现名字

**需求：** 想要在 Vue Devtools 中自定义组件的显示名称，例如将 `Person` 组件显示为 `Person 234`。 **解决方案：** 使用 `vite-plugin-vue-setup-extend` 插件。

![image-20241218212553279](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241218212553279.png)

这样子需要下载

```bash
npm i vite-plugin-vue-setup-extend -D
```

并修改 `vite.config.ts` 里的内容

```bash
import { fileURLToPath, URL } from "node:url";

import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import vueDevTools from "vite-plugin-vue-devtools";
import vueSetupExtend from "vite-plugin-vue-setup-extend"; # 添加这个

// <https://vite.dev/config/>
export default defineConfig({
  plugins: [vue(), vueDevTools(), vueSetupExtend()], #这里添加vueSetupExtend()
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
});
```

**在语法里写 `<script lang="ts" setup name="Person 234">`**

![image-20241218212600418](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241218212600418.png)

## 6.ref 创建：基本类型的响应式数据

> `ref`: 也可以用于对象响应式, 前提必须要加入 `.value`

```html
<template>
  <div class="person">
    <h2>姓名：{{name}}</h2>
    <h2>年龄：{{age}}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">年龄+1</button>
    <button @click="showTel">点我查看联系方式</button>
  </div>
</template>

<script setup lang="ts" name="Person">
  import {ref} from 'vue'
  // name和age是一个RefImpl的实例对象，简称ref对象，它们的value属性是响应式的。
  let name = ref('张三')
  let age = ref(18)
  // tel就是一个普通的字符串，不是响应式的
  let tel = '13888888888'

  function changeName(){
    // JS中操作ref对象时候需要.value
    name.value = '李四'
    console.log(name.value)

    // 注意：name不是响应式的，name.value是响应式的，所以如下代码并不会引起页面的更新。
    // name = ref('zhang-san')
  }
  function changeAge(){
    // JS中操作ref对象时候需要.value
    age.value += 1 
    console.log(age.value)
  }
  function showTel(){
    alert(tel)
  }
</script>
```

> 注意这里 `<script setup lang="ts" name="Person">xxxxx</script>` 里面使用响应式的话必须要写 `.value`

## 7.reactive 创建: 对象类型的响应式数据-013

> `reactive` 只能对象不能基本响应式
>
> 

```vue
<script lang="ts" setup name="Person">
import { reactive, ref } from 'vue';
// 汽车
const car = reactive({ brand: 'BMW', price: 100 })

// 游戏
const games = reactive([
  { id: 'aysdytfsatr01', name: '王者荣耀' },
  { id: 'aysdytfsatr02', name: '原神' },
  { id: 'aysdytfsatr03', name: '三国志' }
])

// 方法
// 对象响应式汽车
const changPrice = () => {
  car.price += 10
}

// 对象响应式游戏
const changFirstGame = () => {
  games[0].name = '蛋仔派对'
}

</script>

<template>
  <div class="person">
    <h2>一辆{{ car.brand }}车,价值{{ car.price }}万</h2>
    <button @click="changPrice">修改汽车价格</button>
    <br>
    <h2>游戏列表</h2>
    <ul>
      <li v-for="g in games" :key="g.id">{{ g.name }}</li>
    </ul>
    <button @click="changFirstGame">修改第一个游戏</button>
  </div>

</template>

<style scoped>
.person {
  background-color: skyblue;
  box-shadow: 0 0 10px;
  border-radius: 10px;
  padding: 20px;
}

button {
  margin: 0 5px;
}

li {
  font-size: 20px;
}
</style>
```

## 8. `ref` 和 `reactive` 区别

1. `ref` 用来定义：**基本类型数据**、**对象类型数据**；

2. `reactive` 用来定义：**对象类型数据**。

- 区别：

1. `ref` 创建的变量必须使用 `.value`（可以使用 `volar` 插件自动添加 `.value`）。 

2. <span style="color:#FF3333;"> `reactive` 重新分配一个新对象，会 **失去** 响应式（可以使用 `Object.assign` 去整体替换）。</span>

- 使用原则：

> 1. **若需要一个 <span style="color:#FF0000;"> 基本类型 </span> 的响应式数据，<span style="color:#FF0000;"> 必须使用 `ref`。</span>**
> 2. **若需要一个 <span style="color:#FF0000;"> 响应式对象 </span>，<span style="color:#FF0000;"> 层级不深，`ref`、`reactive` 都可以。</span>**
> 3. **若需要一个 <span style="color:#FF0000;"> 响应式对象 </span>，<span style="color:#FF0000;"> 且层级较深，推荐使用 `reactive`。</span>**

### 8.1 特殊情况

**场景描述**：

- 一开始你用 `reactive` 定义了一个对象，比如 `car`，用它来存储一些汽车的数据，比如品牌 (`brand`) 和价格 (`price`)。
- 后来，你想从服务器（或者数据库）中拿到一组新的数据，然后把这些新数据更新到 `car` 对象里。

**问题出现**：

- 因为你最开始用的是 `reactive`，Vue 的响应式系统会跟踪 `reactive` 对象的引用和内容。
- 但是如果你直接用 `car = { ... }` 来替换 `car`，Vue 的响应式系统就无法继续跟踪这个新对象了，因为替换掉了整个引用。

**需求总结**：

- 你想让 `car` 保持响应式。
- 你又想从服务器拿到新的数据，并更新到 `car` 上。
- 你需要一种方式，不破坏 Vue 的响应式系统，但又可以把新的数据更新到 `car` 上。

**下面写法不对**:

```vue
<script lang="ts" setup name="Person">
import { reactive, ref } from 'vue';
// 汽车数据
const car = reactive({ brand: 'BMW', price: 100 })
//如果上面这里这样写了,但是我们又想用服务器传回一个数据的话用 reactive 会有严重的错误

const sum = ref(0)

// 方法
// 对象响应式汽车
const changPrice = () => {
  car.price += 10
}

//例如下面:添加了一个修改品牌,
const changbrand = () => {
  car.brand = '梅赛德斯奔驰'
}

// 又突然想拿数据库的数据了
const changCar = () => {
  // car = {brand:'aotuo', price:1} 这里是不行的错误原因请看vue3个人笔记的 8. ref和reactive区别
  Object.assign(car, { brand: 'aotuo', price: 1 })
}

const changSum = () => {
  sum.value += 1
}

</script>

<template>
  <div class="person">
    <h2>一辆{{ car.brand }}车,价值{{ car.price }}万</h2>
    <button @click="changPrice">修改汽车价格</button>
    <button @click="changbrand">修改汽车品牌</button>
    <button @click="changCar">修改汽车</button>
    <br>
    <h2>当期求值为:{{ sum }}</h2>
    <button @click="changSum">点击求值</button>
  </div>

</template>
```

**解决问题**:

`Object.assign(car, { ... })` 的作用就是：**给 `car` 这个对象，换上新的一些“零件”，但保留车子的主体架构不变**。

**举个例子：**

假设 `car` 是一个车子的模型：

```js
const car = reactive({ brand: 'BMW', price: 100 });
```

### **场景 1：**

> **直接换整辆车**

如果你想直接换掉整个车子，比如写：

```js
car = { brand: 'Toyota', price: 50 };
```

Vue 会说：“不行！你不能直接换掉整辆车，因为我已经在监控你原来的那辆车了！如果你直接换车，我会跟丢目标！”

### **场景 2：**

> **只换零件，用 `Object.assign`**

于是我们用 `Object.assign`，这就像是对原车进行 **零件升级**：

```js
Object.assign(car, { brand: 'Toyota', price: 50 });
```

这句话的意思是：不换掉整个车子，而是把车子的品牌改成 “Toyota”，价格改成 `50`。

Vue 看到你只是改了零件，它会说：“没问题！你改车的零件，我还能继续盯着车的变化。”

------

### **总结**

- **不能直接换整个对象**：`car = { ... }` 就像直接换了一辆新车，Vue 追踪不到新的车。
- **可以改对象的内容**：用 `Object.assign`，就像换了车子的某些零件（比如品牌和价格），这样 Vue 还能继续追踪车的变化。

用 `Object.assign(car, { ... })` 的好处就是：让你改内容，不改“壳子”，Vue 的响应式功能不会丢失。



## 9.【toRefs 与 toRef】

> 如果下面的写法按钮修改不会改变,因为只是自己定义的,更改不到`person.name`里的数据
>
> ```js
> const { name, age } = person
> ```
>
> 通过这种方式解构后,`name` 和 `age` 是独立的变量，它们不再是 `person` 的响应式属性，因此修改 `person.name` 或 `person.age` 的值不会影响到模板中使用的 `name` 和 `age`，导致 UI 不会响应式更新。
>
> 但不代表`person.name`不会改变,只是界面ui不改变而已 `console.log(name, person.name);`可以看到`person.name`是一直有改变的

```vue
<script lang="ts" setup>
import { reactive, ref } from 'vue';

//数据
const person = reactive({
  name: '张三',
  age: 18
})

const { name, age } = person

// 方法
const changName = () => {
  person.name += '~'
  console.log(name, person.name);

}
const changAge = () => {
  person.age += 1
}
</script>

<template>
  <div class="person">
    <h2>姓名:{{ name }}</h2>
    <h2>年龄:{{ age }}</h2>
    <button @click="changName">修改名字</button>
    <button @click="changAge">修改年龄</button>
  </div>
</template>
```

![image-20241222161148928](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241222161148928.png)



## 10.**`computed`**计算属性

需求:有一个案例,是要用户单独输入姓和名,然后最后返回全名`(如果是英文名的话第一位要大写)`,并要求有一个按钮进行更改输入框里的数据
如图:

![用户输入并带按钮更改](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241224185143403.png)

**代码实现逻辑**:

1. 定义内容

   - 先定义两个输入框:
     - `firstName`用`v-model`绑定
     - `lastName`用`v-model`绑定

   - 一个全名框:
     - `fullName`

   - 一个点击按钮
     - `changFullName`

2. 构想函数实现方法

   - 定义两个`ref`响应式变量
     - const filterName = **ref**('张')
     - const lastName = **ref**('三')
   - 定义按钮
     - **changFullName**按钮函数
     - **changFullName**按钮方法

3. 注意事项

   1. 按钮方法里的`get`内容有一个没写全,`UI`界面会返回一个信息

      - 如:下面的`toUpperCase()`漏了个括号,`UI`会返回:`图一`

      - ```js
          get() {
            return (
              filterName.value.slice(0, 1).toUpperCase
              + filterName.value.slice(1) +
              '-' +
              lastName.value
            )
          },
        ```

      - ![图一](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241224190328559.png)

   2. 注意:

      - 下面的写法只能读不能更改

      - ```js
        // 这里定义的fullName 是只读的,不能更改
        // 这里是修改了姓和名引起来的重新计算
        const fullName = computed(
            () => {
                return filterName.value.slice(0, 1).toUpperCase() + filterName.value.slice(1) + '-' + lastName.value
            } )
        ```

      - 能改能读的方法只有`get`和`set`配合

      - ```js
        const fullName = computed({
            get() {
                return (
                    filterName.value.slice(0, 1).toUpperCase()
                    + filterName.value.slice(1) +
                    '-' +
                    lastName.value
                )
        
            },
            set(val) {
                const [str1, str2] = val.split('-')
                filterName.value = str1
                lastName.value = str2
        
            }
        })
        ```

**代码如下**:

```vue
<script lang="ts" setup>
import { ref, computed } from 'vue';
const filterName = ref('张')
const lastName = ref('三')

// 按钮方法
const fullName = computed({
  get() {
    return (
      filterName.value.slice(0, 1).toUpperCase()
      + filterName.value.slice(1) +
      '-' +
      lastName.value
    )

  },
  set(val) {
    const [str1, str2] = val.split('-')
    filterName.value = str1
    lastName.value = str2

  }
})

const changFullName = () => {
  fullName.value = 'Li-Si'

}



</script>


<template>
  <div class="person">
    姓: <input type="text" v-model="filterName">{{ }} <br>
    名: <input type="text" v-model="lastName">{{ }} <br>
    全名 : <span>{{ fullName }}</span>
    <button @click="changFullName">将名字更改为li-si</button>
  </div>
</template>

<style scoped>
.person {
  background-color: skyblue;
  box-shadow: 0 0 10px;
  border-radius: 10px;
  padding: 20px;
}
</style>
```

## 11.`watch`监听

- 作用：监视数据的变化（和 `Vue2` 中的 `watch` 作用一致）
- 特点：`Vue3` 中的 `watch` 只能监视以下 **四种数据**：

> 1. <span style="font-weight:bold; color:#FF0000;">`ref` 定义的数据。</span>
> 2. <span style="font-weight:bold; color:#FF0000;">`reactive` 定义的数据。</span>
> 3. <span style="font-weight:bold; color:#FF0000;">函数返回一个值（`getter` 函数）。</span>
> 4. <span style="font-weight:bold; color:#FF0000;">一个包含上述内容的数组。</span>

`Vue3` 中使用 `watch` 的时候，通常会遇到以下几种情况:



### 情况1

监视 `ref` 定义的【基本类型】数据：直接写数据名即可，监视的是其 `value` 值的改变。

**代码实现逻辑**:

1. 定义内容

   - `h2`加插值语法
   - `changeSum`按钮

2. 函数逻辑

   - 定义函数`sum`
   - 定义按钮方法`changeSum`
   - 定义监视函数`stopwatch`

3. 注意`watch`的语法

   - 语法 watch(谁? , 回调函数)

   - ```js
     watch(${1://要监视的函数}, (newValue, oldValue) => {",
           ${2://方法},
           });
     ```

4. 实现逻辑:

   1. 使用了`watch`来监视点击事件`changeSum`
   2. 当点击到了10次会停止对`changeSum`的监视

**代码如下**:

```vue
<script lang="ts" setup>
import { ref, watch } from 'vue';

// 数据：定义 sum 为一个可响应的基本类型
const sum = ref(0);

// 方法：点击按钮使 sum 自增
const changeSum = () => {
  sum.value += 1;
};

// 监视函数：监听 sum 的变化
const stopWatch = watch(sum, (newValue, oldValue) => {
  console.log('sum变化了', newValue, oldValue);
  // 停止监听条件
  if (newValue >= 10) {
    stopWatch(); // 调用返回的函数停止监听
  }
});
</script>

<template>
  <div class="person">
    <h1>情况一: 监视 [ref] 定义的 [基本类型] 数据</h1>
    <h2>当前就和为: {{ sum }}</h2> <!-- 动态展示 sum 的值 -->
    <button @click="changeSum">点击使Sum+1</button>
  </div>
</template>
```

### 情况2

监视 `ref` 定义的【对象类型】数据：直接写数据名即可，监视的是其 `value` 值的改变。

**代码实现逻辑**:

1. 定义内容:

   - `h2`标签:文本
   - `changeName`:修改名字
   - `changeAge`:修改年龄
   - `changePerson`:修改全部信息

2. 函数代码逻辑:

   - `const person`:定义对象数据
   - `changeName`:定义修改名字方法
   - `changeAge`:定义修改年龄方法
   - `changePerson`:定义修改全部信息方法
     - 使用`Object.assign`来重新赋值,:原因方便后期从服务器返回内容

3. `watch`监听方法

   - `deep`使用了深度监视
     - `deep:true`**深度监视**：开启对对象内部属性变化的监听
   - `immediate`使用了立即监视
     - `immediate: true`**立即执行**：确保监听器在组件挂载时立即触发

4. :warning: 注意

   - 情况二: 监视 [ref] 定义的 [对象类型] 数据,

     - <span style="color:#FF0000;">**监视是对象的地址值,若想监视对象内部属性的变化需要开启手动深度监视**</span>

   - 情况二:有一个恶心的问题

     - 如图:![image-20241230155150874](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241230155150874.png)

     - 当点击<span style="color:#FF0000;">`****`</span>或者<span style="color:#FF0000;">`修改年龄`</span>后不管<span style="color:#FF3333;">**`新值`**</span>和<span style="color:#FF0000;">**`旧值`**</span>监听出来的都是一样的

     - 原因如下:

       - 曾经的东西为:

         ```js
         const person = ref({
             name: '张三',
             age: 18
         })
         ```

       - 新的东西为:

         ```js
         Object.assign(person.value, { name: '李四', age: 19 }) 
         ```

         这样子很自然的分清了`新值`和`旧值`的区别

       - 但是如果你`新值`和`旧值`是一个东西,

         - ```js
           const person = ref({
             name: '张三',
             age: 18
           })
           ```

         - 只是在上面的基础进行修改,并没有赋予新的值,如:赋予`李四`

         - 所以呢这当你改动了`张三`变成了`张三~`,但是这里得去监听,因为你监听是已经改了`张三~`所以监听出来的`新旧`值是一样的

5. 总结:

   - 想要区分新旧值，必须让对象本身发生改变（如替换为新对象）。

**代码如下**:

```vue
<script lang="ts" setup>
import { ref, watch } from 'vue';

// 数据
const person = ref({
  name: '张三',
  age: 18
})

// 按钮方法
const changeName = () => {
  person.value.name += '~'
}
const changeAge = () => {
  person.value.age += 1
}
const changePerson = () => {
  // 使用Object.assign(数据对象,{重新赋值}), 方便后期服务器返回
  Object.assign(person.value, { name: '李四', age: 19 }) 
}
// 监视函数
// 语法 watch(谁? , 回调函数)
// const stopWatch = watch(//要监视的函数, (newValue, oldValue) => {
// },{ deep: true, immediate: true });
// deep:true 深度监视 
// immediate: true立即监视 一刷新页面就监视,一开始旧值是空的
// 情况二: 监视 [ref] 定义的 [对象类型] 数据,监视是对象的地址值,若想监视对象内部属性的变化需要开启手动深度监视
watch(person, (newValue, oldValue) => {
  console.log('person变化了', newValue, oldValue)
}, { deep: true, immediate: true }
);


</script>


<template>
  <div class="person">
    <h1>情况二: 监视 [ref] 定义的 [对象类型] 数据</h1>
    <h2>姓名:{{ person.name }}</h2>
    <h2>年龄:{{ person.age }}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">修改年龄</button>
    <button @click="changePerson">修改全部信息</button>
  </div>
</template>

<style scoped>
.person {
  background-color: skyblue;
  box-shadow: 0 0 10px;
  border-radius: 10px;
  padding: 20px;
}
</style>
```

### 情况3

情况三: 监视 [reactive] 定义的 [对象类型] 数据,且默认开启深度监视的,这种深度监视是不可以关闭的

**代码实现逻辑**:

1. 定义内容:
   - `h2`标签:文本
   - `changeName`:修改名字
   - `changeAge`:修改年龄
   - `changePerson`:修改全部信息
2. 函数代码逻辑:
   - `const person`:定义对象数据
   - `changeName`:定义修改名字方法
   - `changeAge`:定义修改年龄方法
   - `changePerson`:定义修改全部信息方法
     - 使用`Object.assign`来**<span style="color:#FF0000;">直接覆盖原值</span>**,:原因方便后期从服务器返回内容
3. `watch`监听方法
   - 当启用[reactive] 的监听,<span style="color:#FF0000;">**会直接默认开启深度监听,并且无法关闭**</span>

**代码如下**:

```vue
<script lang="ts" setup>
    import { reactive, watch } from 'vue';

    // 数据
    const person = reactive({
        name: '张三',
        age: 18
    })

    // 按钮方法
    const changeName = () => {
        person.name += '~'
    }
    const changeAge = () => {
        person.age += 1
    }
    const changePerson = () => {
        // 使用Object.assign(数据对象,{重新赋值}), 方便后期服务器返回
        Object.assign(person, { name: '李四', age: 19 })
    }
    // 监视函数
    // 语法 watch(谁? , 回调函数)
    // const stopWatch = watch(//要监视的函数, (newValue, oldValue) => {
    // },{ deep: true, immediate: true });
    // deep:true 深度监视 
    // immediate: true立即监视 一刷新页面就监视,一开始旧值是空的
    // 情况三: 监视 [reactive] 定义的 [对象类型] 数据,且默认开启深度监视的
    watch(person, (newValue, oldValue) => {
        console.log('person变化了', newValue, oldValue)
    });


</script>


<template>
<div class="person">
    <h1>情况三: 监视 [reactive] 定义的 [对象类型] 数据</h1>
    <h2>姓名:{{ person.name }}</h2>
    <h2>年龄:{{ person.age }}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">修改年龄</button>
    <button @click="changePerson">修改全部信息</button>
    </div>
</template>

<style scoped>
    .person {
        background-color: skyblue;
        box-shadow: 0 0 10px;
        border-radius: 10px;
        padding: 20px;
    }
</style>
```

### 情况4

监视 `ref` 或 `reactive` 定义的【对象类型】数据中的 **某个属性**，注意点如下：

1. 若该属性值 **不是**【对象类型】，需要写成函数形式。![image-20241230193618879](https://gitee.com/ActonT/pic-go_img/raw/master/image-20241230193618879.png)
2. 若该属性值是 **依然** 是【对象类型】，可直接编，也可写成函数，建议写成函数。

结论：监视的要是对象里的属性，那么最好写函数式，注意点：若是对象监视的是地址值，需要关注对象内部，需要手动开启深度监视。

**代码实现逻辑**:

1. 定义内容:

   - `h2`标签文本
   - `changeName`修改名字, `changeAge`修改年龄 , `changeC1`修改第一坐骑 , `changeC2`修改第二坐骑, `changeCar`修改整个坐骑

2. 函数代码逻辑

   - `person`定义数据,
     - 里面包括一个`car`的对象数据
   - `changeName`修改名字, `changeAge`修改年龄 , `changeC1`修改第一坐骑 , `changeC2`修改第二坐骑, `changeCar`修改整个坐骑为`雅迪`和`九号`

3. 监听函数

   - 这里只想监听`name`的话需要给一个箭头函数,:

   - 语法

     - // 语法 watch((谁? :  这里使用箭头函数包裹例如: <span style="color:#FF0000;">**() => { return (person.name) }**</span>), 回调函数)

     ```js
     watch(() => { return (person.name) }, (newValue, oldValue) => {
         console.log('person值变化了:', newValue, oldValue)
     });
     
     //简写
     watch(() => person.name, (newValue, oldValue) => {
         console.log('person值变化了:', newValue, oldValue)
     });
     ```

   - 监听数据里的对象

     - ```js
       // 下面的是监视对象的形式
       watch(() => person.car, (newValue, oldValue) => {
         console.log('car值变化了:', newValue, oldValue)
       }, {
         deep: true
       });
       ```

     - 注意:这里如果写了`person.car`还不行,只能监听导里面细致的变化,整体不可以例如:

     - <img src="https://gitee.com/ActonT/pic-go_img/raw/master/监视.gif" alt="监视" style="zoom:50%;" />

     - 下面的是带有深度监听的`deep: true`,当加了这个,`car`对象里的细节与整体都会被监听到,如下:

       <img src="https://gitee.com/ActonT/pic-go_img/raw/master/监视2.gif" alt="监视2" style="zoom:50%;" />

**代码如下**:

```vue
<script lang="ts" setup>
    import { reactive, watch } from 'vue';

    // 数据
    const person = reactive({
        name: '张三',
        age: 18,
        car: {
            c1: '奔驰',
            c2: '宝马'
        }
    })

    //方法

    const changeName = () => {
        person.name += '~'
    }
    const changeAge = () => {
        person.age += 1
    }
    const changeC1 = () => {
        person.car.c1 = '奥迪'
    }
    const changeC2 = () => {
        person.car.c2 = 'BBA'
    }
    const changeCar = () => {
        person.car = { c1: '雅迪', c2: '九号' }
    }

    // 监听name的get函数


    // 监听
    // // 监视,不是对象类型的,需要转换成函数
    // watch(() => { return (person.name) }, (newValue, oldValue) => {
    //   console.log('person值变化了:', newValue, oldValue)
    // });

    // 下面的是监视对象的形式,可以直接写,也能写成函数,更推荐写函数
    watch(() => person.car, (newValue, oldValue) => {
        console.log('car值变化了:', newValue, oldValue)
    }, {
        deep: true
    });
</script>


<template>
<div class="person">
    <h1>情况四: 监视 [reactive] 定义的 [对象类型] 数据</h1>
    <h2>姓名:{{ person.name }}</h2>
    <h2>年龄:{{ person.age }}</h2>
    <h2>坐骑:{{ person.car.c1 }}.{{ person.car.c2 }}</h2>
    <hr>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">修改年龄</button>
    <button @click="changeC1">修改第一坐骑</button>
    <button @click="changeC2">修改第二坐骑</button>
    <button @click="changeCar">修改整个坐骑</button>
    </div>
</template>

<style scoped>
    .person {
        background-color: skyblue;
        box-shadow: 0 0 10px;
        border-radius: 10px;
        padding: 20px;
    }
</style>
```

### 情况五

监视上述的多个数据

要在监视的位置前面动态加函数，根据是否是对象判断是否需要添加函数，可以优化为以下通用的逻辑：

```js
/**
 * 动态监视指定位置的值变化
 * @param {string} keyPath - 要监视的属性路径 (支持嵌套对象，如 "obj.prop1.prop2")
 * @param {function} callback - 当值发生变化时调用的回调函数，传入新值和旧值
 */
function monitorValue(keyPath, callback) {
    const pathParts = keyPath.split('.'); // 分割路径
    let obj = window; // 默认从全局对象 window 开始
    let key = pathParts.pop(); // 属性名是路径的最后一部分

    // 遍历路径，找到对象
    pathParts.forEach(part => {
        if (!obj[part]) obj[part] = {}; // 若路径中有 undefined，则初始化为空对象
        obj = obj[part];
    });

    let currentValue = obj[key]; // 当前值

    if (typeof currentValue === 'object' && currentValue !== null) {
        console.warn(`"${keyPath}" 是一个对象，监视其本身不需要添加函数逻辑。`);
        return; // 对象无需动态添加函数
    }

    // 动态定义 getter 和 setter
    Object.defineProperty(obj, key, {
        configurable: true,
        enumerable: true,
        get() {
            return currentValue;
        },
        set(newValue) {
            const oldValue = currentValue;
            currentValue = newValue;
            console.log(`监视到 "${keyPath}" 值变化:`, { oldValue, newValue });
            callback(newValue, oldValue); // 触发回调函数
        }
    });
}

// 示例：监视一个变量的值变化
monitorValue('someVar', (newValue, oldValue) => {
    console.log(`someVar 已改变，从 ${oldValue} 变为 ${newValue}`);
});

// 示例：监视嵌套对象的属性
monitorValue('nested.obj.prop', (newValue, oldValue) => {
    console.log(`nested.obj.prop 已改变，从 ${oldValue} 变为 ${newValue}`);
});

// 示例：修改值以触发监视
window.someVar = 42; // 触发监视
window.nested = { obj: { prop: 123 } }; // 初始化嵌套对象
window.nested.obj.prop = 456; // 触发监视

```

**例子代码如下**:

```vue
<template>
  <div class="person">
    <h1>情况五：监视上述的多个数据</h1>
    <h2>姓名：{{ person.name }}</h2>
    <h2>年龄：{{ person.age }}</h2>
    <h2>汽车：{{ person.car.c1 }}、{{ person.car.c2 }}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">修改年龄</button>
    <button @click="changeC1">修改第一台车</button>
    <button @click="changeC2">修改第二台车</button>
    <button @click="changeCar">修改整个车</button>
  </div>
</template>

<script lang="ts" setup name="Person">
  import {reactive,watch} from 'vue'

  // 数据
  let person = reactive({
    name:'张三',
    age:18,
    car:{
      c1:'奔驰',
      c2:'宝马'
    }
  })
  // 方法
  function changeName(){
    person.name += '~'
  }
  function changeAge(){
    person.age += 1
  }
  function changeC1(){
    person.car.c1 = '奥迪'
  }
  function changeC2(){
    person.car.c2 = '大众'
  }
  function changeCar(){
    person.car = {c1:'雅迪',c2:'爱玛'}
  }

  // 监视，情况五：监视上述的多个数据
  watch([()=>person.name,person.car],(newValue,oldValue)=>{
    console.log('person.car变化了',newValue,oldValue)
  },{deep:true})

</script>
```

## watchEffect

`watchEffect`会全自动的监视不会去指定,会便捷很多

```vue
<script lang="ts" setup>
import { ref, reactive, watch, watchEffect } from 'vue';
// 按钮禁用
const isDisabled1 = ref(false)
const isDisabled2 = ref(false)

// 数据

const temp = ref(10)
const height = ref(0)

// 方法

const changeTemp = () => {
  temp.value += 10
}
const changeHeight = () => {
  height.value += 10
}

// 监视
// watch(() => [temp.value, height.value], (value) => {
//   const [newTemp, newHeight] = value
//   let alertMessage = '';
//   if (newTemp >= 60) {
//     alertMessage += `水温报警:${temp.value}℃`
//     isDisabled1.value = true
//     console.log(alertMessage + '给服务器发送数据')
//   }
//   if (newHeight >= 80) {
//     alertMessage += `水位报警:${height.value}℃`
//     isDisabled2.value = true
//     console.log(alertMessage + '给服务器发送数据')
//   }
//   if (!alertMessage) {
//     console.log('一切正常');
//   }
// });

// watchEffect
watchEffect(() => {
  if (temp.value >= 60 || height.value >= 80) {
    console.log('警报')
  }
})

</script>


<template>
  <div class="person">
    <h2>需求:当水温达到60度,或水位达到80cm时,给服务器发送请求</h2>
    <h2>当前水温:{{ temp }}℃</h2>
    <h2>当前水位: {{ height }}cm</h2>
    <button :disabled="isDisabled1" @click="changeTemp">水温+10</button>
    <button :disabled="isDisabled2" @click="changeHeight">水位+10</button>
  </div>
</template>

<style scoped>
.person {
  background-color: skyblue;
  box-shadow: 0 0 10px;
  border-radius: 10px;
  padding: 20px;
}

button {
  margin: 5px;
}
</style>
```

## 标签的ref属性

> * 用在普通 `DOM` 标签上，获取的是 `DOM` 节点。
>
> * 用在组件标签上，获取的是组件实例对象。

### 错误例子

错误例子:`person`

```vue
<script lang="ts" setup>
import { ref } from 'vue';

//按钮
const showLog = () => {
  console.log(document.getElementById('title2'))
}
</script>

<template>
  <div class="person">
    <h1>中国</h1>
    <h2 id="title2">北京</h2>
    <h3>AhNan</h3>
    <button @click="showLog">点击输出元素</button>
  </div>
</template>
```

> `console.log(document.getElementById('title2'))`:调用浏览器里的js,进行获取`DOM`元素,然后`console.log`打印到控制台

错误例子:`App.vue`

```vue
<template>
  <h2>你好</h2>
  <Person />
</template>


<script lang="ts" setup>
import Person from './components/Person.vue';
</script>

```

> 这样子会导致,`App.vue`先比`person`优先显示

### 正确例子

> 需要先创建`title2`的容器用于存储`ref`标记的内容

正确例子:`person` 

```vue
<script lang="ts" setup>
import { ref } from 'vue';

//按钮
const showLog = () => {
  console.log(document.getElementById('title2'))
}
//创建一个title2,用于存储ref标记的内容
const title2 = ref()

</script>


<template>
  <div class="person">
    <h1>中国</h1>
    <h2 ref="title2">北京</h2>
    <h3>AhNan</h3>
    <button @click="showLog">点击输出元素</button>
  </div>
</template>
```



正确例子:`App.vue`

```vue
<script lang="ts" setup>
import { ref } from 'vue';

//按钮
const showLog = () => {
  console.log(title2.value)
}
//创建一个title2,用于存储ref标记的内容
const title2 = ref()

</script>


<template>
  <div class="person">
    <h1>中国</h1>
    <h2 ref="title2">北京</h2>
    <h3>AhNan</h3>
    <button @click="showLog">点击输出元素</button>
  </div>
</template>
```

![image-20250103165306769](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250103165306769.png)

**下面为局部样式**: 

![image-20250103165420127](https://gitee.com/ActonT/pic-go_img/raw/master/image-20250103165420127.png)
