# Vue 重学

## 基本命令

> 在 JavaScript（和 Vue）中，`!this` 是一种逻辑操作符的使用方式。下面是详细的解释：
>
> ------
>
> ### **1. `!` 是逻辑非（NOT）操作符**
>
> - ! 用来取反，即将一个值的布尔状态反转：
>   - 如果值是 `true`，取反后变成 `false`。
>   - 如果值是 `false`，取反后变成 `true`。
>
> ------
>
> ### **2. `this` 是什么？**
>
> - 在 Vue 中，`this` **指向当前组件的实例**。
> - 当你访问数据或方法时，用 `this` 来引用，例如 `this.isActive` 访问组件内的 `isActive` 属性。
>
> ------
>
> ### **3. 那么 `!this` 是什么？**
>
> 在 Vue 或 JavaScript 中，直接使用 `!this` 其实并不常见。我们通常会用 `!this.someProperty` 来反转一个布尔值的状态。
>
> - **示例：**
>   当你在方法中使用 `this.isActive`，`isActive` 是个布尔值（`true` 或 `false`）。
>   使用 `!this.isActive` 就是 **取反操作**，如果原值是 `true`，那么它会变成 `false`；如果原值是 `false`，就会变成 `true`。




## 创建项目命令

```bash
npm init vue@latest
```

## Vue 项目: 目录结构

```bash
.vscode ---Vscode工具文件:删除都没问题
node_modules ---Vue项目的运行依赖文件夹
public ---资源文件夹:浏览器图标
src ---源码文件夹
	assets ---公共资源文件夹
	main.js ---资源文件夹
.gitignore  ---git:团队协作文件
index.hetml  ---入口的html文件
package.json ---项目版本,依赖等
README.md  ---注释文件
Vite.config.js ---Vue配置文件
```

## Vue 模板语法

### 文本插值

```vue
<template>
<h3>模板语法</h3>
<P>
    {{msg}}
    </P>
</template>

<script>
export default{
    data(){
        return{
            msg:"神奇的语法",
        }
    }
}
</script>

```

## 使用 Javascript

```vue
<template>
<h3>模板语法</h3>
<P>
    {{msg}}
    </P>

<p>
    {{number + 1}} 
    //输入11
    </p>

<p>
    {{ok ? 'YES' : 'NO'}}
    //ok为true显示ok
    </p>

<p>
    {{message.split('').reverse().join('')}}
        值   空字符串切割     翻转数组 合并起来      
    </p>
</template>

<script>
export default{
    data(){
        return{
            msg:"神奇的语法",
            number:10,
            ok:true,
            message:"大家好",
        }
    }
}
</script>

```

## 原始 HTML

双花括号不是插入 html, 是直接文本, 想要渲染 heml 的得用 v-html

```vue
<template>
<h3>模板语法</h3>
<P>
    {{msg}}
    </P>

<p>
    {{number + 1}} 
    //输入11
    </p>

<p>
    {{ok ? 'YES' : 'NO'}}
    //ok为true显示ok
    </p>

<p>
    {{message.split('').reverse().join('')}}
        值   空字符串切割     翻转数组 合并起来      
    </p>
</template>

<script>
export default{
    data(){
        return{
            msg:"神奇的语法",
            number:10,
            ok:true,
            message:"大家好",
            rawHtml:"<a href='https://itbaizhan.com>百战程序员</a>'"
        }
    }
}
</script>

```

## 属性绑定

双花括号不能在 HTML 属性(attributes)中使用, 要想绑定属性需要用到 v-bind 指令

```bash
用法
<div v-bind:class="msg">xxx</div>
```

```vue
<template>
<div v-bind:class="msg">
    Text
    </div>

</template>

<script>
export default{
    data(){
        return{
			msg:"appclass"
        }
    }
}
</script>

```

### 注意

如果绑定的是 null 或 undefined, 那么该属性会被直接移除

```vue
<template>
<div v-bind:class="msg" v-bind:title="a">
    Text
    </div>

</template>

<script>
export default{
    data(){
        return{
			msg:"appclass"
            a:null
        }
    }
}
</script>

```

![image-20241026195338256](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241026195338256.png)

### 简写

```vue
直接写 :class="appclass" 这个写法等于 v-bind:class="appclass"
```

### 布尔值

用按钮来做例子, 如果用户操作正确按钮就可以点击

disabled 可以让按钮不可点击

```vue
<template>

<button :disabled="isButtonDisabled">
    Button
    </button>

</template>

<script>
export default{
    data(){
        return{
			isButtonDisabled:false //为不可点击,如果为true就可以
        }
    }
}
</script>

```

### 动态绑定多个值

```vue
<template>

<div :="objectOfAttrs">
    测试
    </div>

</template>

<script>
export default{
    data(){
        return{
			calss:"appclass",
            id:"appid",
        }
    }
}
</script>

```

![image-20241026200307375](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241026200307375.png)

## 条件渲染

- v-if
- v-else
- v-else-if
- v-show

比较复杂的用 v-if 或 v-else-if

```vue
<template>
    <h3>条件渲染</h3>
    <div v-if="flag">你能看到我吗?</div>
    <div v-else>那你还是看看我吧!</div>
    <!-- 如果type等等于a就显示a以此类推,不等于的话就显示Not A/B/C -->
    <div v-if="type === 'A'">A</div>
    <div v-else-if="type ==='B'">B</div>
    <div v-else-if="type ==='C'">C</div>
    <div v-else>Not A/B/C</div>
</template>

<script>
export default {
    data() {
        return{
            flag:true,
            // 如果为真就显示,为假的的就不显示
            type:"D"
        }
    },
}
</script>
```

### v-show

如果是简单的判断的话用 v-show, 减少体积

```vue
    <!-- v-show,只能判断自己本身,没有判断功能 -->
    <div v-show="flag">你能看到我吗?</div>
```

### v-if vs v-show

有较高渲染开销的是 v-show

有较高的切换渲染开销是 v-if

![image-20241026204242879](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241026204242879.png)

## 列表渲染

v-for

```vue
<template>
    <h3>列表渲染</h3>
    <!-- 利用v-for来渲染下面的列表, -->
     <p v-for="item in names">{{item}}</p>
</template>

<script>
export default {
    data() {
        return{
            names:["啊南","啊东","啊北"]
        }
    },
}
</script>
```



![image-20241026210457137](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241026210457137.png)

### 动态循环

动态的话要用上 v-bind

![image-20241026211015097](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241026211015097.png)

### 下标索引

索引元素

```vue
<template>
    <h3>列表渲染</h3>
     <!-- 下面的是显示出下标索引 -->
     <p v-for="(item,index) in names">{{item}}-{{ index }}</p>
</template>

<script>
export default {
    data() {
        return{
            names:["啊南","啊东","啊北"]
        }
    },
}
</script>
```

![image-20241026211415689](C:/Users/MAH/AppData/Roaming/Typora/typora-user-images/image-20241026211415689.png)

### 更接近 javascript 迭代的是用 of

in 和 of 都可以看个人习惯

```vue
<div v-for="item of items">
    sss
</div>
```

### v-for 遍历对象

```vue
<template>
    <h3>列表渲染</h3>
     <!-- 用v-for遍历对象 -->
      <div>
        <p v-for="item of userInfo">{{ item }}</p>
        <!-- 这里的意思是item遍历userInfo对象里面的所有值 -->
      </div>
</template>

<script>
export default {
    data() {
        return{
            userInfo:{
                name:'Ah Nan',
                age:'19',
                sex:'男'
            }
        }
    },
}
</script>
```

## v-for 的 key 值

vue 默认是按照 0 开始更新渲染, 当数据顺序变成了打乱的, vue 不会移动到某个元素会继续执行就地更新

```bash
123  ---vue会重0开始,0是第一位
132  ---vue还是会重0开始
如果我们要132输出成123就是要加key值
```

```vue
<template>
    <h3>Key属性添加到v-for中</h3>
    <p v-for="(item,index) in names" :key="index">{{ item }}</p>
//为了某些数据打乱了不按照本地更新,减少消耗要把index放进动态的:key里
//:key  === v-bind:key
</template>


<script>

export default{
    data(){
        return{
            names:['百战程序员','尚学堂','IT']
        }
    }
}

</script>
```

```vue
真的开发用数据的唯一索引成为key值是最好的
<template>
    <div>
      <h3>Key 属性添加到 v-for 中</h3>
      <p v-for="(item, index) in names" :key="index">{{ item }}</p>
  
      <h3>使用 v-for 遍历 JSON 数据</h3>
      <div v-for="entry in result" :key="entry.id">
        <h4>{{ entry.title }}</h4>
        <img :src="entry.avator" alt="avatar" style="width:100px; height:auto;" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        names: ['百战程序员', '尚学堂', 'IT'],
        result: [
          {
            id: 2261677,
            title: "鄂尔多斯 | 感受一座城市的璀璨夜景 感受一座城市，除了日里里的车水马龙，喧嚣繁华",
            avator: "https://pic.qyer.com/avatar/002/25/77/30/200?v=1560226451"
          }
        ]
      };
    }
  };
  </script>  
```

## 事件处理

- v-on 指令来用于监听事件, 简写为@

```vue
on:click="methodName" 或 @click="handler"
```

### 内联事件处理器

简单场景

```vue
<template>
    <h3>内联事件处理器</h3>
    <button v-on:click="count++">Add</button>
    <!-- //click点击 -->
    <p>{{ count }}</p>
</template>

<script>
export default{
    data(){
        return{
            count:0,
        };
    }
};
</script>
```

### 方法事件处理器

```vue
<template>
    <h3>方法事件处理器</h3>
    <button @click="addCount">Add</button>
    <p>{{ count }}</p>
</template>

<script>
export default{
    data(){
        return{
            count:0,
        };
    },
    //所有的方法或函数都方methods里
    methods:{
        addCount(){
            // 用this读到count时就++
            // ++ :自加1
            this.count++
            console.log("点击了")
        }
    }
};
</script>
```

### 事件参数

事件参数可以获取 `event` 对象和通过事件传递数据

```vue
<template>
    <h3>事件参数</h3>
    <button @click="addCount">Add</button>
    <p>{{ count }}</p>
</template>

<script>
export default{
    data(){
        return{
            count:0,
        };
    },
    //所有的方法或函数都方methods里
    methods:{
        addCount(e){
            // 获取event对象
            // vue中的event对象就是原生JS的event对象
            this.count++
            e.target.innerHTML =  "Add"+this.count
        }
    }
};
</script>
```

### 传递参数

```vue
<template>
    <h3>事件参数</h3>
    <button @click="addCount(hello)">Add</button>
    <p>{{ count }}</p>
</template>

<script>
export default{
    data(){
        return{
            count:0,
        };
    },
    //所有的方法或函数都方methods里
    methods:{
        addCount(msg){
            // 获取event对象
            // vue中的event对象就是原生JS的event对象
            // e.target.innerHTML =  "Add"+this.count
            console.log(msg)
            // 上面事件中加了个hello当我们点击按钮时后台会获得一个hello
            this.count++
        }
    }
};
</script>
```

### 例子

点数组里面的名, 用 console 后台获取点到的名

```vue
<template>
    <h3>例子传参</h3>
    <!-- 传参中获取e用$event -->
    <p @click="getNameHandler(item,$event)" v-for="(item,index) of names" :key="index">{{ item }}</p>
</template>

<script>
export default{
    data(){
        return{
			names:["iwen","ime","frank"],
        };
    },
    //所有的方法或函数都方methods里
    methods:{
        // 传参过程获取event
        getNameHandler(name,e){
            console.log(name);
            console.log(e)
        }
    },
};
</script>
```

### 事件修饰符

常见的默认修饰符

- `.stop`   ---
- `prevent`   ---阻止默认事件
- `.once`   ---
- `enter`   ---



### 阻止事件

```vue
<template>
    <h3>事件修饰符</h3>
    <h3>阻止修饰符</h3>
    <a @click.prevent="clickHandle" href="https://itbaizhan.com">百战程序员</a>
    <!-- // a标签不跳转打印点击了, -->

</template>

<script>

    export default{
        data(){
            return{

            }
        },
        methods:{
            clickHandle(e){
                // 阻止默认事件
                // e.preventDefault();
                console.log("点击了")
            }
        }
    };


</script>
```



### 阻止事件冒泡

```vue
<template>
    <h3>事件修饰符</h3>
    <h3>阻止修饰符</h3>
    <div @click="clickDiv">
        <p @click.stop="clickP">测试冒泡</p>
        <!-- 如果不写.stop,当点击事件时,父级div也会显示出来,加了.stop就不会显示父级div -->
    </div>
</template>

<script>

    export default{
        data(){
            return{

            }
        },
        methods:{
            clickDiv(){
                console.log("DIV");
                
            },
            clickP(){
                console.log("P");
                
            }
        }
    };


</script>
```

## 数组变化侦听

### 变化更新

下面的都会直接引发界面的更新

- `push`
- `pop`
- `shift`
- `unshift`
- `splice`
- `sort`
- `reverse`

```vue
<template>
    <h3>数组侦听变化</h3>
    <button @click="addListHandle">添加数据</button>
    <ul>
        <li v-for="(item,index) of names" :key="index">{{ item }}</li>
    </ul>
</template>

<script>

    export default{
        data(){
            return{
                names:["iwen","ime","frank"],
            }
        },
        methods:{
            addListHandle(){
            // 添加数据，并且更新页面显示
            this.names.push("HaiKuAI");
            }

        },
    };

</script>
```



### 替换一个数组

下面的不会引发界面的更新而是新给一个数组

- `filter`
- `concat`
- `slice`

```vue
<template>
    <h3>数组侦听变化</h3>
    <button @click="addListHandle">添加数据</button>
    <ul>
        <li v-for="(item,index) of names" :key="index">{{ item }}</li>
    </ul>
</template>

<script>

    export default{
        data(){
            return{
                names:["iwen","ime","frank"],
            }
        },
        methods:{
            addListHandle(){
            // 合并一个数组
            this.names.concat(["HaiKuAI"])
            console.log(this.names.concat(["HaiKuAI"]));
            
            }

        },
    };

</script>
```

### 后续可能会长用 concat

想要用 concat 还要发生变化的话

需要给用 concat 的重新赋值

```vue
<template>
    <h3>数组侦听变化</h3>
    <button @click="addListHandle">添加数据</button>
    <ul>
        <li v-for="(item,index) of names" :key="index">{{ item }}</li>
    </ul>
</template>

<script>

    export default{
        data(){
            return{
                names:["iwen","ime","frank"],
            }
        },
        methods:{
            addListHandle(){
            // 添加数据，并且更新页面显示
            // this.names.push("HaiKuAI");
            // 合并一个数组
            this.names = this.names.concat(["HaiKuAI"])
            
            }

        },
    };

</script>
```

## 计算属性

​    <!-- <p>{{ itbaizhan.content.length > 0 ? 'Yes' :'No' }}</p> -->
普通的这样写表达式虽然没问题但是以后会很麻烦, 如果多起来的话, 也会比较臃肿, 所以我们直接用 vue 提供的计算属性来实现

```vue
<template>
    <h3>{{itbaizhan.name }}</h3>
    <!--计算属性 -->
    <!-- <p>{{ itbaizhan.content.length >0 ? 'Yes' :'No' }}</p> -->
     <p>{{ itbaizhanContent }}</p>
</template>

<script>
    export default{
        data(){
            return{
               itbaizhan:{
                name:"百战程序员",
                content:['java','前端','python']
            }
            }
        },
        // 计算属性
        computed:{
            itbaizhanContent(){
                return this.itbaizhan.content.length > 0 ? 'yes':'No'
            }
        }
    };

</script>
```

### 计算属性 VS 方法

用方法也可以实现这个计算功能,

**`计算属性`**: 如果计算的东西不发生变化, 用计算属性会减少臃肿, 可以被调用多次

**`方法`**: 如果计算放在方法里面, vue 他会每调用一次都会计算, 这样可能会造成体积臃肿

很少纯在计算经常改变

___

## Class 绑定

在标签里 class 后面可以方数组也可以方对象

### Class 绑定对象

```vue
<template>
    <p :class="{'active':isActive}">Class样式绑定</p>
</template>

<script>
export default{
    data(){
        return{
            isActive:true
            //这里true决定下面样式显示还是不显示
        }
    },
};
</script>

<style>
.active{
    color: gray;
    font-size: 30px;
}
</style>
```

### 多个对象绑定形式

如果对象很多, 上面的绑定方法过于繁琐会很长

所以用下面的多个对象绑定

1 和 2 都是一样的用多个对象绑定会简洁很多

```vue
<template>
    <p :class="{'active':isActive,'text-danger':hasError}">Class样式绑定1</p>
    <p :class="classObject">Class样式绑定2</p>
</template>

<script>
export default{
    data(){
        return{
            isActive:true,
            hasError:true,
            classObject:{
                'active':true,
                'text-danger':true
            }
        }
    },
};
</script>

<style>
.active{
    font-size: 30px;
}
.text-danger{
    color: gray;   
}
</style>
```

### 数组绑定

 ```vue
 <template>
     <p :class="[arrActive,arrHasError]">Class样式绑定2</p>
 </template>
 
 <script>
 export default{
     data(){
         return{
             arrActive:"active",
             arrHasError:"text-danger"
         }
     },
 };
 </script>
 
 <style>
 .active{
     font-size: 30px;
 }
 .text-danger{
     color: gray;   
 }
 </style>
 ```

### 数组和对象

> **温馨提示**
>
> **数组和对象嵌套过程中, 只能数组去嵌套对象, 就是 [{}], 不能{[]}**

```vue
<template>
<div :class="[{'active':isActive},errorClass]"></div>
</template>

<script>
export default{
    data(){
        return{
            isActive:true,
            errorClass:"text-danger"
        }
    }
}
</script>
```

## style 绑定

```vue
<template>
  <div>
    <!-- 这里是动态样式的绑定 -->
    <h3 :style="titleStyle">计算属性绑定样式</h3>

    <!-- 这个按钮用来切换样式，每点一下就改变标题样式 -->
    <button @click="toggleStyle">切换样式</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true, // 用一个布尔值来控制样式，true 表示激活状态，false 表示未激活
    };
  },
  computed: {
    // 计算属性 titleStyle：根据 isActive 的值返回不同的样式
    titleStyle() {
      return {
        color: this.isActive ? "green" : "black", 
          // 如果 isActive 是 true，就用绿色；否则用黑色
        fontWeight: this.isActive ? "bold" : "normal",
          // 激活时文字加粗，不激活时正常
      };
    },
  },
  methods: {
    // 方法 toggleStyle：用来切换 isActive 的值
    toggleStyle() {
      this.isActive = !this.isActive;
        // 每次点击按钮都反转 isActive 的值
    },
  },
};
</script>
```

## 侦听器

每次变化的东西都可以用侦听器来看到变化

但是用 watch 的话函数名必须与侦听的数据对象保持一致

```vue
<template>
    <h3>侦听器</h3>
    <p>{{ message }}</p>
    <button @click="updatahandle">修改数据</button>
</template>

<script>
export default{
    data(){
       return{
         message:"Hello!"
       } 
    },
    methods:{
        updatahandle(){
            this.message = "Word!"
        }
    },
    //侦听器
    watch:{
        // newValue 改变后的值
        // oldValue 改变前的值
        message(newValue,oldValue){
            console.log(newValue,oldValue);
            
        }
    }
};

</script>
```

___

## 表单输入绑定

`v-model`: 用户输入后下面直接显示

```vue
<template>
    <form>
        <input type="text" v-model="message">
        <p>{{ message }}</p>
        <!-- 这里的message可以直接显示用户输入的东西 -->
    </form>
</template>

<script>
export default{
    data(){
        return{
            message:"",
        }
    }
}
</script>
```

## 模板语法

获取 DOM 操作

> DOM 白话文解释
>
> **HTML 就像页面的骨架，DOM 是它的翻译版本**
>
> - 浏览器会把你的 **HTML** 代码解析成 **DOM 树**，你可以想象这是一棵带有分支的“家谱树”，每个 HTML 元素（如 `div`、`p`、`h1` 等）都是这棵树的一个节点。
>
> **你可以通过 JavaScript 操作 DOM**
>
> - DOM 提供了对 HTML 元素的访问，比如：
>   - **查找某个元素**：找到页面上的按钮。
>   - **修改元素的内容**：更改 `<p>` 里的文字。
>   - **给元素添加事件**：让按钮能被点击时触发某个函数

> **`this.$refs`** 是 Vue 提供的获取 DOM 元素的方式之一，它让我们可以直接访问模板中的 HTML 元素或组件实例。这类似于 **原生 JavaScript 的 DOM 操作**，

```vue
<template>
  <!-- 使用 ref 给 HTML 元素加标识，方便在 JS 中获取 -->
  <div ref="container" class="container">{{ content }}</div>

  <!-- 输入框，用 ref 绑定，方便在 JS 中读取输入的值 -->
  <input type="text" ref="username" />

  <!-- 按钮，点击时调用 getElementHandle 方法 -->
  <button @click="getElementHandle">获取元素</button>
</template>

<script>
export default {
  data() {
    return {
      content: "内容", // 定义 content 变量，初始值是"内容"
    };
  },
  methods: {
    // 这个方法在点击按钮时执行
    getElementHandle() {
      // 用 this.$refs 获取绑定了 ref="container" 的元素
      // 修改该元素的 innerHTML 属性，把内容变成 "哈哈哈"
      this.$refs.container.innerHTML = "哈哈哈";

      // 获取 ref="username" 的输入框的 value 值，并在控制台打印出来
      console.log(this.$refs.username.value);
    },
  },
};
</script>
```

___

## 组件组成

- **`scoped` 让当前样式只在当前组件中生效**

### App.vue 组件引用过程

```vue
<template>
  <!-- 显示组件 -->
<MyComponcnt/>
</template>

<script>
// 第一步:引入组件
import MyComponcnt from './components/MyComponcnt.vue';
// 第二步:注入组件
export default{
  components:{
    MyComponcnt
  }
}

</script>

<style>

</style>
```

___

### 组件传参

在 Vue 中，组件之间传递数据非常常见。我们可以通过 **父组件传数据给子组件**（`props`）和 **子组件把数据传回父组件**（`$emit`）来实现。下面用白话文一步步帮你理解。

------

### **父组件传递参数给子组件（`props`）**

#### **概念：**

父组件可以把数据传给子组件，想象成 **父母给孩子买零花钱**，父母（父组件）可以直接给钱（数据），然后孩子（子组件）接收并使用。

------

#### **代码示例：**

**父组件（Parent.vue）：**

```vue
 <template>
  <div>
    <h3>父组件</h3>
    <!-- 使用子组件，并传递一个名字作为参数 -->
    <ChildComponent :name="userName" />
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue'; // 导入子组件

export default {
  data() {
    return {
      userName: "小明", // 定义一个数据：用户名
    };
  },
  components: {
    ChildComponent, // 注册子组件
  },
};
</script>
```

**子组件（ChildComponent.vue）：**

```vue
 <template>
  <div>
    <h4>子组件</h4>
    <p>接收到的用户名：{{ name }}</p>
  </div>
</template>

<script>
export default {
  props: {
    //name:"字符串"
    name: String, // 子组件声明一个 "name" 属性，表示接收的数据类型是字符串
  },
};
</script>
```

#### **白话解释：**

- **父组件** 通过 `:name="userName"` 传递数据给子组件，就像 **给孩子零花钱**。
- **子组件** 用 `props` 声明自己需要的参数（`name`），然后直接用 `{{ name }}` 显示出来。

------

### **子组件传递数据回父组件（`$emit`）**

#### **概念：**

子组件有时需要把信息传给父组件，比如 **孩子把零花钱用完了**，告诉父母自己需要更多零花钱。

------

#### **代码示例：**

**父组件（Parent.vue）：**

```vue
 <template>
  <div>
    <h3>父组件</h3>
    <p>收到的子组件消息：{{ message }}</p>
      <!-- sendMessage 是子组件触发的自定义事件。-->
	  <!--父组件通过 @sendMessage 来监听这个事件。-->
    <ChildComponent @sendMessage="receiveMessage" />
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  data() {
    return {
      message: "", // 用于保存来自子组件的信息
    };
  },
  components: {
    ChildComponent,
  },
  methods: {
    // 接收来自子组件的信息
	// 这里msg同等与message写法风格问题
    receiveMessage(msg) {
      this.message = msg; // 将子组件传来的信息保存到父组件的数据中
    },
  },
};
</script>
```

**子组件（ChildComponent.vue）：**

```vue
 <template>
  <div>
    <h4>子组件</h4>
    <button @click="sendMessageToParent">给父组件发送消息</button>
  </div>
</template>

<script>
export default {
  methods: {
    // 子组件方法，用于发送消息
    sendMessageToParent() {
      this.$emit('sendMessage', '你好，父组件！'); // 使用 $emit 发送消息给父组件
    },
  },
};
</script>
```

#### **白话解释：**

- **子组件** 用 `$emit` 把消息（`"你好，父组件！"`）发送给父组件，就像 **孩子告诉父母：我要零花钱**。
- **父组件** 通过监听事件（`@sendMessage`），接收子组件传过来的消息，并在 `receiveMessage` 方法里处理。



___



### **1. 传参的默认值（default）**

#### **概念：**

- 如果父组件没有传递某个 `prop` 参数，Vue 会使用子组件中定义的 **默认值**。

#### **代码示例：**

**子组件 (ChildComponent.vue)：**

```vue
<template>
  <div>
    <h3>用户名：{{ name }}</h3>
  </div>
</template>

<script>
export default {
  props: {
    // 定义 name 这个 prop，并设置默认值为 '游客'
    name: {
      type: String,
      default: '游客',
    },
  },
}; 
</script>
```

#### **父组件 (Parent.vue)：**

```vue
<template>
  <div>
    <ChildComponent /> <!-- 没传参数，默认显示 '游客' -->
    <ChildComponent name="小明" /> <!-- 传入参数，显示 '小明' -->
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent,
  },
};
</script>
```

#### **白话解释：**

- 如果 **父组件没有传 `name` 参数**，子组件会使用默认值 **'游客'**。
- 如果 **父组件传递了 `name` 参数**，子组件就会使用传入的值，比如显示 `'小明'`。

------

### **2. 必须参数（required）**

#### **概念：**

- 有些参数是组件正常运行所 **必须的**，如果父组件没有传递这些参数，Vue 会在控制台给出警告。

#### **代码示例：**

**子组件 (ChildComponent.vue)：**

```vue
 <template>
  <div>
    <h3>用户名：{{ name }}</h3>
  </div>
</template>

<script>
export default {
  props: {
    // 定义 name 这个 prop，并且标记为必须传递
    name: {
      type: String,
      required: true,
    },
  },
};
</script>
```

#### **父组件 (Parent.vue)：**

```vue
 <template>
  <div>
    <ChildComponent name="小明" /> <!-- 正常使用 -->
    <ChildComponent /> <!-- 如果不传 name，控制台会出现警告 -->
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent,
  },
};
</script>
```

#### **白话解释：**

- **`name` 是必须的参数**，如果父组件没有传递它，Vue 会在控制台打印出 **警告信息**。
- 这就像一个人 **必须有身份证**，否则办不了某些业务。

------

### **3. 默认值与必须参数一起使用**

有时候，我们既想设置默认值，又希望确保父组件明确传递某些参数。这时可以结合 **`required`** 和 **`default`**。

#### **示例代码：**

```vue
 <script>
export default {
  props: {
    age: {
      type: Number,
      required: true, // 这个参数必须传
    },
    name: {
      type: String,
      default: '游客', // 如果父组件没有传 name，就使用默认值
    },
  },
};
</script>
```

#### **白话解释：**

- **`age` 是必须的参数**，如果不传就会警告。
- **`name` 有默认值**，父组件没传也不会报错，默认显示 `'游客'`





## 组件事件配合 v-model

**父级**

 ```vue
 <template>
     <h3>Main</h3>
     <p>搜索内容为:{{ search   }}</p>
 <!--这里就直接返回储存的新数据-->
     <SearchEvent @searchEvent="getSearch"/>
 <!--父组件通过 @searchEvent="getSearch" 监听子组件发送的 searchEvent 事件。-->
 </template>
 
 <script>
 // 引入子级
 import SearchEvent from './SearchEvent.vue';
 export default{
 
     data(){
         return{
             search:""
         }
 
     },
     components:{
         // 注册
         SearchEvent
     },
     //这边父级每次都在收取这个数据然后返回到上面data里的search里储存起来
     methods:{
         getSearch(data){
             this.search=data
         }
     }
 }
 </script>
 ```

**子级**

```vue
<template>
   搜索: <input type="text" v-model="search">
</template>

<script>
export default{
    emits: ['searchEvent'],
    data(){
        return{
            search:""
            
        }
    },
    //一旦监听器,监听到search有新的变化,那就实时的返回给父级发送新的数据
    watch:{
        search(newvalue,oldvalut){
            this.$emit("searchEvent",newvalue)
        }
        
    }
}
</script>
```

## 组件数据传递

`props` **也可以实现子传父**

需要通过一些类型传输, 通过函数类型

`ComponentA`

```vue
<template>

<h3>
    ComponentA
    </h3>
<ComponentB title="标题" :onEvent="dataFn"/>
<p>
    {{message}} <!--直接在页面上显示子传父的数据-->
    </p>
</template>

<script>
    import ComponentB from "./ComponentB.vue"
    export default {
        data(){
            return{
                message:""
            }
        },
        components:{
            ComponentB
        },
        methods:{
            dataFn(data){
                this.message = data //这里是将子传父得到的数据存放于message

            }
        }

</script>
```

`ComponentB`

```vue
<template>

<h3>
    ComponentB
    </h3>
<p>
    {{title}}<!---//子传父--->
    </p>
<p>
    {{onEvent('传递数据')}}
    </p>
</template>

<script>
    import ComponentB from "./ComponentB.vue"
    export default {
        data(){
            return{

            }
        },
        props:{
            title:String, //接收字符串类型
            onEvent:Function, //接收函数类型,用于子传父
        }

    }

</script>
```

## 插槽

组件用来接收模板内容, 模板内容 html 结构

`Slots`

### 子组件 SlotsDemo.vue

```vue
<template>
  <div>
    <h3>插槽基础知识</h3>
    <slot></slot> <!-- 这里是插槽，父组件的内容会被放在这里 -->
  </div>
</template>
```

> **`<slot>` 标签** 就是一个占位符，它表示："这里以后会有东西传进来，具体传什么由父组件决定"。
>
> **`<h3>`** 是固定显示的内容，说明这部分是子组件的样式或结构。
>
> `<slot>` 会把父组件传来的内容放在这个标签的位置上。
>
> 可以将 `slot` 摆上 h3 上面, 然后就可以在插槽基础知识上显示

```bash
插槽标题
插槽内容
插槽基础知识
```

### 父组件 APP.vue

```vue
<template>
<div>
    <SlotsDemo>
        <div>
            <h3>插槽标题</h3>
            <p>插槽内容</p>
    </div>
    </SlotsDemo>
    </div>
</template>

<script>
    import SlotsDemo from './components/SlotsDemo.vue';

    export default {
        components: {
            SlotsDemo, // 这里是注册刚刚那个子组件
        },
    };
</script>
```

父组件通过 `<SlotsDemo>` 使用了子组件，并在 `<SlotsDemo>` 的标签之间塞了内容：

```vue
<div>
    <h3>插槽标题</h3>
    <p>插槽内容</p>
</div>
```

这些内容会被传到子组件的 `<slot>` 位置上。所以，当你运行这段代码时，页面会显示如下：

```bash
插槽基础知识
插槽标题
插槽内容
```

> ### **简单比喻：**
>
> **假设你去餐厅吃饭，`<slot>` 就是服务员留给你自己带酒水的地方。服务员给你准备了餐具和菜（子组件固定的部分），但你可以自带红酒（父组件传入的内容）。这样每个顾客都能根据自己的需求决定餐桌上放什么。**

### 插槽(续)

显示动态插槽文件

**父组件代码**

```vue
<template>
<div>
    <SlotsDemo2>
        <h3>{{ message }}</h3> <!-- 动态插槽内容 -->
    </SlotsDemo2>
    </div>
</template>

<script>
    import SlotsDemo2 from './components/SlotsDemo2.vue';

    export default {
        data() {
            return {
                message: "插槽内容续集" // 这是父组件中的数据
            };
        },
        components: {
            SlotsDemo2 // 注册子组件
        },
    };
</script>

```

**子组件：SlotsDemo2.vue**

```vue
<template>
<div>
    <h3>插槽基础知识</h3>
    <slot></slot> <!-- 插槽占位符 -->
    </div>
</template>

<script>
    export default {
        name: 'SlotsDemo2'
    };
</script>

```

> ### **动态插槽的工作原理**
>
> 1. **插槽占位符 `<slot>`**：
>    在 `SlotsDemo2` 组件中，`<slot>` 就是个占位符，专门等着父组件传递内容。
> 2. **父组件传递动态内容**：
>    父组件中把 `{{ message }}` 包在 `<h3>` 里传给子组件。而这个 `message` 是绑定在 `data` 中的，可以随时更新。
> 3. **动态更新内容**：
>    当你改变 `message` 的值（比如从 `"插槽内容续集"` 改为 `"新的内容"`），插槽里的显示内容也会自动变成 `"新的内容"`。

### 插槽默认值

如果插槽内容没有给, 那么可以在显示插槽的那个文件的 `<slot><slot>` 里写个插槽默认值

```vue
<template>
<div>
    <h3>插槽基础知识</h3>
    <slot>默认值</slot> <!-- 这里插槽没有数据的化会显示这个默认值,有数据的话就会显示数据 -->
    </div>
</template>

<script>
    export default {
        name: 'SlotsDemo2'
    };
</script>
```

### 具名插槽

`v-slot` 可以用 `#` 号来代替简写方式

通过名字来渲染插槽, 避免重复渲染

**子组件：SlotsDemo.vue**

```vue
<template>
<div>
    <slot name="header"></slot>  <!-- 头部插槽 -->
   <!--- 预留了一个 具名插槽，等父组件往这个“头部”插槽里塞内容。--->
    <slot></slot>  <!-- 默认插槽 -->
    </div>
</template>

```

父组件：使用 `v-slot`

```vue
<template>
  <SlotsDemo>
    <template v-slot:header>
        <!--父组件用 v-slot:header 声明这是给 header 插槽的内容-->
      <h1>这里是标题</h1>
    </template>
    <p>这里是默认插槽内容</p>
  </SlotsDemo>
</template>

```

### 作用域插槽

有时候可能会同时使用父级和子级的数据, 需要把子组件数据传给插槽, 这样子先要把数据传给父级

后在放到父元素里的内容, 后父元素一个给 slot

**子组件（SlotsDemo.vue）**

```vue
<template>
  <div>
    <slot :user="user"></slot>  <!-- 传递 user 对象给父组件 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: { name: "小明", age: 20 }  // 子组件中的数据
    };
  }
};
</script>

```

> 子组件定义了一个 `<slot>`，并通过 **`:user="user"`** 向父组件传了一个 `user` 对象。



**父组件**：**接收子组件的数据**

```vue
<template>
<SlotsDemo v-slot="slotProps">
    <!---slotProps包含了子组件的:user = "user"--->
    <p>用户名：{{ slotProps.user.name }}</p>
    <p>年龄：{{ slotProps.user.age }}</p>
    </SlotsDemo>
</template>

<script>
    import SlotsDemo from './components/SlotsDemo.vue';

    export default {
        components: { SlotsDemo }
    };
</script>

```

**页面效果：**

```bash
用户名：小明  
年龄：20
```

![image-20241028200047160](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241028200047160.png)

## 组件生命周期

从开始创建到销毁的过程就是生命周期

> **<span style="color:#6600CC;"> 生命周期函数 </span>**
>
> - <span style="color:#FF3399;"> 创建期: `beforeCreate` `created` </span>
> - <span style="color:#FF3399;"> 挂载期: `beforeMounte` `mounted` </span>
> - <span style="color:#FF3399;"> 更新期: `beforeUpdate`   `updated` </span>
> - <span style="color:#FF3399;"> 销毁期: `beforeUnmount`  ` unmounted `</span>

### 生命周期应用场景

> 没有初始化前获取不到数据所以 beforeCreate 会显示空的
>
> create 就可以, 不过建议使用 mounted 因为数据是其次的结构是优先的

```vue
<template>
<h3>组件生命周期函数应用</h3>
<p ref="name">百战程序员</p>
<ul>
    <li v-for="(item,index) of banner" :key="(index)">
        <h3>{{ item.id}}</h3>
        <p>{{ item.name }}</p>
        <p>{{ item.email }}</p>
    </li>
</ul>
</template>

<script>
export default{
    // 网络请求渲染前,先初始化
    data(){
        return{
            banner:[]

        }
    },
    create(){
        //创建之后:模拟网络请求
        // 是可以显示的,但是用beforeCreate就是不可以的会显示空
        this.banner=[
                        {
                            "id": 1,
                            "name": "Alice",
                            "email": "alice@example.com"
                        },
                        {
                            "id": 2,
                            "name": "Bob",
                            "email": "bob@example.com"
                        },
                        {
                            "id": 3,
                            "name": "Charlie",
                            "email": "charlie@example.com"
                        }
            ]
    },
    beforeCreate() {
         console.log(this.$refs.name); //undefined
     },
    mounted() {
         console.log(this.$refs.name); // 正常获取到 DOM 元素
     },
}
</script>
```

## 动态组件

> 有的时候会需要在两个组件中来回切换, 也有可能不止两个组件,

`<component :is=""` 来使组件来回切换

```vue
<template>

 <!-- <UserComponent/> -->
  <component :is="tabComponent"></component>
  <button @click="changeHandle">切换组件</button>

</template>


  <script>
import ComponentA from './components/ComponentA.vue';
import ComponentB from './components/ComponentB.vue';
export default{
  data(){
    return{
      tabComponent:"ComponentA"
    }
  },
  components:{
    ComponentA,
    ComponentB
  },
  methods:{
    changeHandle(){
      // 判断如果tabComponent等等于A的话就切换成B,等等B的话就切换给A,然后赋值给data下的Component
      this.tabComponent = this.tabComponent  == "ComponentA" ? "ComponentB" : "ComponentA"
    }
  }
}

</script>
```

#### **`<component :is="tabComponent">`：**

- 这个 Vue 特有的 `<component>` 标签会根据 `tabComponent` 的值来动态渲染不同的组件。
- 例如：
  - 如果 `tabComponent` 的值是 `"ComponentA"`，就显示 `ComponentA` 组件。
  - 如果是 `"ComponentB"`，就显示 `ComponentB` 组件



## 组件保持存活

`<component :is=""` 当使用这个时, 被切换的组件就会被卸载, 就是当 A 换到 B 时 A 就会被卸载

可以通过 `keep-alive` 使组件强制被切换并且保持存活

`ComponentA`

```vue
<template>
    <h3>ComponentA</h3>
  </template>
  
  <script>
  export default {
    name: 'ComponentA',
    created() {
      console.log('ComponentA 被创建（第一次加载）');
    },
    mounted() {
      console.log('ComponentA 已挂载到页面');
    },
    unmounted() {
      console.log('ComponentA 被销毁');
    },
  };
  </script>
 
```

`ComponentB`

```vue
<template>
    <h3>ComponentB</h3>
  </template>
  
  <script>
  export default {
    name: 'ComponentB',
    created() {
      console.log('ComponentB 被创建（第一次加载）');
    },
    mounted() {
      console.log('ComponentB 已挂载到页面');
    },
    unmounted() {
      console.log('ComponentB 被销毁');
    },
  };
  </script>
 
```

`APP.vue`

```vue
<template>
  <keep-alive>
    <component :is="tabComponent"></component>
  </keep-alive>
  <button @click="changeHandle">切换组件</button>
</template>

<script>
import ComponentA from './components/ComponentA.vue';
import ComponentB from './components/ComponentB.vue';

export default {
  data() {
    return {
      tabComponent: 'ComponentA', // 初始值为 ComponentA
    };
  },
  components: {
    ComponentA,
    ComponentB,
  },
  methods: {
    changeHandle() {
      const oldComponent = this.tabComponent;
      // 切换组件
      // 判断如果tabComponent等等于A的话就切换成B,等等B的话就切换给A,然后赋值给data下的Component
      this.tabComponent = this.tabComponent === 'ComponentA' ? 'ComponentB' : 'ComponentA';
      const newComponent = this.tabComponent;

      // 追踪组件切换时的变化
      console.log(`切换组件：从 ${oldComponent} 切换到 ${newComponent}`);
    },
  },
};
</script>

```

![image-20241029115605366](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241029115605366.png)

> 可以看到组件切换后并没有被销毁

## 异步组件

### **同步组件：**

- 同步组件是指 **所有的组件都会在页面加载时一起加载**，无论是否立刻使用它们。这意味着页面加载时，**所有引用的组件都会在一开始被加载到内存中**。
- 换句话说，**即使某些组件暂时没被显示**（比如切换组件的场景），它们也已经全部加载完成了，等到需要时就会显示出来，而不再重新加载。

**图片中同步组件的情况：**

- 你说的图片显示的是同步组件，这意味着所有的组件都已经加载好了，不会有新的组件在后面延迟加载。
- 也就是说，所有组件按照顺序在初次加载时都已经加载完毕，等待展示。
- ![image-20241029120212745](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20241029120212745.png)

------

### **异步组件：**

- **异步组件** 的核心特性是 **延迟加载**。它允许你将组件分割成独立的“模块”，并且 **只在需要的时候才去加载** 它们。
- 通过 `defineAsyncComponent` 方法，可以创建一个异步组件。在初始加载时，这些组件不会被立即加载，而是只有在它们真正被使用的时候才加载。
- 这种方式有助于提高性能，尤其是在大型应用中，因为它可以减少初始页面的加载时间，只加载当前页面需要的部分，后续再按需加载其它组件。

**异步组件的执行过程**：

- 当你定义一个异步组件时，Vue 会在渲染它时去请求和加载它。如果你有多个异步组件，它们会同时开始加载，而不是按顺序逐个加载。
- 这意味着多个异步组件可以并行下载并渲染，而不是一个完成后才开始下一个。

> 下面图片是同步组件, 所有组件都加载了, 没有再新的

```VUE
<template>
  <keep-alive>
    <component :is="tabComponent"></component>
  </keep-alive>
  <button @click="changeHandle">切换组件</button>
</template>

<script>
import { defineAsyncComponent } from 'vue';
import ComponentA from './components/ComponentA.vue';
// import ComponentB from './components/ComponentB.vue';

// 异步组件
const ComponentB = defineAsyncComponent(() =>
  import('./components/ComponentB.vue')
);

export default {
  data() {
    return {
      tabComponent: 'ComponentA', // 初始值为 ComponentA
    };
  },
  components: {
    ComponentA,
    ComponentB,
  },
  methods: {
    changeHandle() {
      const oldComponent = this.tabComponent;
      // 切换组件
      // 判断如果tabComponent等等于A的话就切换成B,等等B的话就切换给A,然后赋值给data下的Component
      this.tabComponent = this.tabComponent === 'ComponentA' ? 'ComponentB' : 'ComponentA';
      const newComponent = this.tabComponent;

      // 追踪组件切换时的变化
      console.log(`切换组件：从 ${oldComponent} 切换到 ${newComponent}`);
    },
  },
};
</script>

```

## 依赖注入

> 可以直接从父级传数据给孙级,
>
> 正常是只能一级一级传的, 限制可以直接用 `provide` 和 `inject` 来解决这个问题

```vue
// ParentComponent.vue
<template>
  <div>
    <h2>我是父组件</h2>
    <ChildComponent />
  </div>
</template>

<script>
import { defineComponent, provide } from 'vue';
import ChildComponent from './ChildComponent.vue';

export default defineComponent({
  name: 'ParentComponent',
  components: {
    ChildComponent,
  },
  setup() {
    const data = '来自父组件的数据';
    provide('parentData', data);
  },
});
</script>

// ChildComponent.vue
<template>
  <div>
    <h3>我是子组件</h3>
    <GrandchildComponent />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import GrandchildComponent from './GrandchildComponent.vue';

export default defineComponent({
  name: 'ChildComponent',
  components: {
    GrandchildComponent,
  },
});
</script>


GrandchildComponent.vue
<template>
  <div>
    <h4>我是孙组件</h4>
    <p>{{ injectedData }}</p>
  </div>
</template>

<script>
import { defineComponent, inject } from 'vue';

export default defineComponent({
  name: 'GrandchildComponent',
  setup() {
    const injectedData = inject('parentData');
    return {
      injectedData,
    };
  },
});
</script>
```

> 这个穿透传递只能由大传给小, 小不能传给大

## 公共资源文件夹 assets

`assets` 为了存放图片等公共的东西

## Axios 网络请求

### 1. 什么是 Axios？

`Axios` 是一个用来发 **网络请求** 的工具，简单来说，它帮你在前端和服务器之间传递数据。比如，我们常需要去服务器拿一些数据（GET 请求），或者把表单数据传到服务器（POST 请求），用 Axios 都能很方便地做到。

### 2. 怎么在 Vue 项目里用 Axios？

如果你是用 Vue CLI 创建的项目，可以用 npm 来安装 Axios。

```bash
npm install axios
```

### 3. 引入 Axios

有了 Axios 后，你可以在 Vue 的组件里用它。

```vue
 // 在组件里引入 axios
import axios from 'axios';
```

### 4. GET 请求示例（拿数据）

假设你要从一个 API 地址拿用户列表数据。

```vue
 export default {
  data() {
    return {
      users: []  // 用来存放用户数据
    };
  },
  mounted() {
    // 组件加载后发起 GET 请求
    axios.get('https://jsonplaceholder.typicode.com/users')
      .then(response => {
        this.users = response.data;  // 把请求到的数据保存到 users 里
      })
      .catch(error => {
        console.error('请求失败：', error);  // 如果失败，输出错误信息
      });
  }
};
```

**解释**：

- `.get()` 是用来发送 GET 请求的。
- `response.data` 是服务器返回的数据。
- `.catch()` 是用来捕获请求失败时的错误。

### 5. POST 请求示例（传数据）

如果你想把一个用户提交的表单数据传到服务器，可以这样用：

```vue
 export default {
  data() {
    return {
      newUser: {
        name: '',
        email: ''
      }
    };
  },
  methods: {
    submitForm() {
      axios.post('https://jsonplaceholder.typicode.com/users', this.newUser)
        .then(response => {
          console.log('用户创建成功：', response.data);
        })
        .catch(error => {
          console.error('请求失败：', error);
        });
    }
  }
};
```

**解释**：

- `.post()` 是发送 POST 请求的，它的第一个参数是 API 地址，第二个参数是你要传的数据。
- 服务器返回的数据会在 `.then()` 里处理，如果有错误就会跑到 `.catch()` 里。

### 6. 设置全局 Axios（偷懒用法）

如果你很多地方都要用 Axios，可以把它设置成全局的，这样每个组件都能直接用。

在 `main.js` 里这样写：

```vue
 import axios from 'axios';
import Vue from 'vue';

Vue.prototype.$axios = axios;
```

然后在组件里直接用：

```
 this.$axios.get('https://jsonplaceholder.typicode.com/users')
  .then(response => {
    console.log(response.data);
  });
```

### 7. 请求头、拦截器（进阶）

有时候需要给请求加一些特殊信息，比如认证 token，这可以通过请求头来实现：

```
axios.defaults.headers.common['Authorization'] = 'Bearer your_token';
```

还可以用拦截器在请求或响应前做些处理：

```
//发送请求拦截器
axios.interceptors.request.use(config => {
  console.log('请求发送前做点什么');
  // config包含了网络请求的所有信息
  return config;
}, error => {
  return Promise.reject(error);
});

//响应拦截器
axios.interceptors.response.use(response => {
  console.log('响应数据处理：', response);
  return response;
}, error => {
  return Promise.reject(error);
});
```

> 带状态

```vue
// 在 main.js 或 axios 的配置文件中设置拦截器
import axios from 'axios';

// 请求拦截器（可选，如果需要统一处理请求前操作）
axios.interceptors.request.use(config => {
  console.log('请求发送前处理');
  return config;
}, error => {
  return Promise.reject(error);
});

// 响应拦截器：捕获不同的状态码错误
axios.interceptors.response.use(
  response => {
    // 如果请求成功，直接返回数据
    return response;
  },
  error => {
    // 如果请求失败，根据状态码处理
    const { response } = error;

    if (response) {
      switch (response.status) {
        case 400:
          console.error('错误请求：请检查输入。');
          alert('错误请求：请检查输入。');
          break;
        case 401:
          console.error('未授权：请登录。');
          alert('未授权：请登录。');
          break;
        case 403:
          console.error('禁止访问：权限不足。');
          alert('禁止访问：权限不足。');
          break;
        case 404:
          console.error('资源未找到。');
          alert('资源未找到。');
          break;
        case 500:
          console.error('服务器错误，请稍后再试。');
          alert('服务器错误，请稍后再试。');
          break;
        default:
          console.error(`未知错误：${response.status}`);
          alert(`未知错误：${response.status}`);
      }
    } else {
      console.error('网络异常或服务器未响应。');
      alert('网络异常或服务器未响应，请检查网络。');
    }

    // 将错误返回，以便组件还能继续处理它
    return Promise.reject(error);
  }
);

```

