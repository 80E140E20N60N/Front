# VUE

## Vue

### 指令

#### 赋值{{ }}

#### v-model

#### v-for

#### v-on 

#### v-if

#### v-show

#### computed计算属性

#### watch



### 组件之间传值

#### 父传子

使用[props](https://cn.vuejs.org/v2/guide/components.html#Prop)，父组件可以使用props向子组件传递数据。

```js
<template>
    <child :msg="message"></child>
</template>

<script>

import child from './child.vue';

export default {
    components: {
        child
    },
    data () {
        return {
            message: 'father message';
        }
    }
}
</script>

------------------------------------------------------------------------------------
@## child.vue

<template>
    <div>{{msg}}</div>
</template>

<script>
export default {
    props: {
        msg: {
            type: String,
            required: true
        }
    }
}
</script>
```



#### 子传父

子组件通过$emit触发事件，回调给父组件。

```js
<template>
    <child @msgFunc="func"></child>
</template>

<script>

import child from './child.vue';

export default {
    components: {
        child
    },
    methods: {
        func (msg) {
            console.log(msg);
        }
    }
}
</script>

------------------------------------------------------------------------------------
@## child.vue

<template>
    <button @click="handleClick">点我</button>
</template>

<script>
export default {
    props: {
        msg: {
            type: String,
            required: true
        }
    },
    methods () {
        handleClick () {
            //........
            this.$emit('msgFunc');
        }
    }
}
</script>
```



#### 兄弟组件传值

可以新建一个实例作为中央事件总线



### 路由

#### 创建

####配置

####嵌套路由

####编程式导航



### 脚手架

####main.js





### 修饰符

#### .sync

.sync是vue中用于实现简单的“双向绑定”的语法糖：vue的prop是单向下行绑定：父级的prop的更新会向下流动到子组件中，但是反过来不行。当需要对prop进行“双向绑定”时，就可以用.sync来解决

用法：

```js

父组件：   .sync
<text-document :title.sync="doc.title"></text-document>

------------------------------------------------------------------------------------

子组件：   update
当子组件需要更新 title 的值时，它需要显式地触发一个更新事件：
this.$emit('update:title', newValue)

```







## Vuex

### 基本信息

Vuex:统一管理各组件共享数据的工具

共享数据存在state中，可以在组件中引用显示，state中的状态不能直接修改，只能借助mutations进行修改。mutations只能执行同步操作，执行异步操作需要通过actions，然后将数据提交给mutations进行修改

[![https://gitee.com/ZZH6/picture/raw/master/Vue/4UOE!^!ok078.png](https://gitee.com/ZZH6/picture/raw/master/Vue/4UOE!^!ok078.png)](https://gitee.com/ZZH6/picture/raw/master/Vue/4UOE!^!ok078.png)

### state

存放数据且可以显示在组件中    

直接使用方式：this.$store.state.count

注：状态对象，一般在data或computed里使用

### getters

获取数据的辅助方法,用来优化state中的数据显示,方便使用

注：一般在data或computed里使用

### mutations   

1、凡是修改state状态，都是通过mutations来完成

2、mutations只能执行同步操作

3、直接使用方式  this.$store.commit(' addCount ',123)

注：改变数据的函数，一般在method里使用

### actions

1、actions可以执行异步操作

2、actions不可以直接操作state的状态，需要使用mutations才可以

3、会返回一个promise，即可以接.then

4、直接使用方式  this.$store.dispatch(' setAsyncCount ',123)

注：异步函数，一般在method里使用

```js
state:{
	count:0
},

mutations{
	addCouunt(state,data){
		state.count += data
	}
},

actions:{
	setAsyncCount(store,data){
		setTimeout(()=>{
			store.commit('addCouunt',data)
		},3000)
	}
}

//可以看出mutations内的函数自带state参数,可以对state状态进行修改
//而actions内的函数自带store参数,可以调用mutations内的函数对state状态进行修改
```

### 模块化 Module

命名空间namespaced

默认情况下action、mutation 和 getter 是注册在**全局命名空间**，如果想要保持内部模块的高封闭性，可以在各模块设置命名空间锁（ namespaced: true），写法如下

```js
user: {
  namespaced: true,
   state: {
     token: ''
   },
   mutations: {
    //  这里的state表示的是user的state
     updateToken (state，data) {
        state.token = data
     }
   }
},
```
带有命名空间锁的模块使用方式：

```js
<button @click="test">修改token</button>

//注：需要带上模块的文件路径
   //直接写法
  methods: {
       test () {
           this.$store.commit('user/updateToken',res.data.data)
       }
   }	

  //辅助函数写法
 import { mapMutations } from 'vuex'
  methods: {
       ...mapMutations(['user/updateToken']),
       test () {
           this['user/updateToken'](res.data.data)
       }
   }
```

