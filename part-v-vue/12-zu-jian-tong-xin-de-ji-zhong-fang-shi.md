# vue组件通信的几种方式

## 一，父子组件传值

### 1，父组件向子组件传值

* 创建子组件，在src/components/文件夹下新建一个Child.vue , Child.vue的中创建props，然后创建一个名为message的属性

```js
<template>
    <div id="container">
        {{messgae}}
    </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  props: {
    message: {
      type: String,
      default: ""
    }
  }
};
</script>
<style scoped>
#container{
    color: red;
    margin-top: 50px;
}
</style>
```

* 在父组件中注册Child组件，并在template中加入child标签，标签中添加message属性并赋值

```js
<template>
    <div id="container">
        <input type="text" v-model="text" @change="dataChange">
        <!--1,动态绑定-->
        <Child :message="text"></Child>
        <!--2，标签中添加message属性并赋值-->
        <Child message="父组件的值"></Child>
    </div>
</template>
<script>
import Child from "@/components/Child";
export default {
  data() {
    return {
      text: "父组件的值"
    };
  },
  methods: {
    dataChange(data){
        this.message = data
    }
  },
  components: {
    Child
  }
};
</script>
<style scoped>
</style>
```

总结：

1. 子组件在props中创建一个属性，用以接收父组件传过来的值

2. 父组件中注册子组件

3. 在子组件标签中添加子组件props中创建的属性

4. 把需要传给子组件的值赋给该属性

### 2，子组件向父组件传值或更新父组件值

* 在子组件中创建一个按钮，给按钮绑定一个点击事件 , 在响应该点击事件的函数中使用$emit来触发一个自定义事件，并传递一个参数

```js
<template>
    <div id="container">
        <input type="text" v-model="msg">
        <button v-on:click="sendMsgToParent">传递到父组件</button>
    </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "传递给父组件的值"
    };
  },
  props:{},
  methods: {
    sendMsgToParent:function() {
      this.$emit("listenToChildEvent", this.msg);
    }
  }
};
</script>
<style scoped>
#container {
  color: red;
  margin-top: 50px;
}
</style>
```

* 在父组件中的子标签中监听该自定义事件并添加一个响应该事件的处理方法

```js
<template>
    <div id="container">
        <Child @getData="getData" v-on:listenToChildEvent="showMsgFromChild"></Child>
        <p>{{msg}}</p>
    </div>
</template>
<script>
import Child from "@/components/Child";
export default {
  data() {
    return {
      msg: "父组件默认值"
    };
  },
  methods: {
    getData(data) {
      this.msg = data;
    },
    showMsgFromChild(data){
        console.log(data);
    }
  },
  components: {
    Child
  }
};
</script>
<style scoped>
</style>
```

总结：

1. 子组件中需要以某种方式例如点击事件的方法来触发一个自定义事件

2. 将需要传的值作为$emit的第二个参数，该值将作为实参传给响应自定义事件的方法

3. 在父组件中注册子组件并在子组件标签上绑定对自定义事件的监听

4. 在通信中，无论是子组件向父组件传值还是父组件向子组件传值，他们都有一个共同点就是有中间介质，子向父的介质是自定义事件，父向子的介质是props中的属性。抓准这两点对于父子通信就好理解了

## 二，平级组件进行通信

### 1， 利用总线方式可以平级组件进行通信

无论是父向子传值还是子向父传值，都需要一个中间介质。对于平级组件来说其实也一样，他们也需要一个中间介质来作为一个中央事件总线

* 我们先来创建中央事件总线，在src/assets/下创建一个eventBus.js内容如下

```js
import Vue from 'Vue'
export default new Vue ;
```

```
   eventBus中我们只创建了一个新的Vue实例，以后它就承担起了组件之间通信的桥梁了，也就是中央事件总线
```

* 创建一个firstChild组件，引入eventBus这个事件总线，接着添加一个按钮并绑定一个点击事件

```js
<template>
    <div id="firstChild">
         <h2>firstChild组件</h2>
        <button v-on:click="sendMsg"></button>
    </div>
</template>
<script>
import bus from "../assets/eventBus";
export default {
  methods: {
      sendMsg:function(){
          bus.$emit("userDefinedEvent","this messgae is from firstChild");
      }
  }
};
</script>
<style scoped>
</style>
```

我们在响应点击事件的sendMsg函数中用$emit触发了一个自定义的userDefinedEvent事件，并传递了一个字符串参数,ps:$emit实例方法触发当前实例\(_这里的当前实例就是bus_\)上的事件,附加参数都会传给监听器回调。

* 我们再创建一个secondChild组件，引入eventBus事件总线，并用一个p标签来显示传递过来的值

```js
<template>
    <div id="secondChild">
         <h2>secondChild组件</h2>
         <p>从firstChild组件中接受的字符串：{{msg}}</p>
    </div>
</template>
<script>
import bus from "../assets/eventBus";
export default {
  data(){
      return {
          msg:"默认值"
      }
  },
  mounted: {
      sendMsg:function(){
          var self=this;
          bus.$on("userDefinedEvent",function(msg){
              self.msg=msg;
          });
      }
  }
};
</script>
<style scoped>
</style>
```

我们在mounted中，监听了userDefinedEvent,并把传递过来的字符串参数传递给了$on监听器的回调函数PS:mounted:是一个Vue生命周期中的钩子函数，简单点说就类似于jquery的ready，Vue会在文档加载完毕后调用mounted函数。$on:监听当前实例上的自定义事件\(此处当前实例为bus\)。事件可以由$emit触发，回调函数会接收所有传入事件触发函数\($emit\)的额外参数。

* 在父组件中，注册这两个组件，并添加这两个组件的标签

```js
<template>
    <div id="app">
         <firstChild></firstChild>
         <secondChild></secondChild>
    </div>
</template>
<script>
    import firstChild from "../components/firstChild";
    import secondChild from "../components/secondChild";
    export default {
        name:'app',
        components:{
            firstChild,
            secondChild
        }
    }
</script>
```

总结：

1. 创建一个事件总线，例如demo中的eventBus，用它作为通信桥梁

2. 在需要传值的组件中用bus.$emit触发一个自定义事件，并传递参数

3. 在需要接收数据的组件中用bus.$on监听自定义事件，并在回调函数中处理传递过来的参数

## 三, vue高级组件之provide/inject

### 1,provide/inject

provide / inject 是 2.2 新增的方法，可以以一个祖先组件向所有子孙后代注入依赖（一个内容）。provider/inject：简单的来说就是在父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量。以上两者可以在父组件与子组件、孙子组件、曾孙子...组件数据交互，也就是说不仅限于prop的父子组件数据交互，只要在上一层级的声明的provide，那么下一层级无论多深都能够通过inject来访问到provide的数据

1. 父级组件如下

```js
<template>
<div class="test">
<son prop="data"></son>
</div>
</template>
<script>
export default {
    name: 'Test',
    provide: {
     name: 'Garrett'
    }
}
</script>
```

2.孙子组件，注意这里是孙子组件，父级 -&gt; 子组件 -&gt; 孙子组件三个层级关系

```js
<template>
<div>
{{name}}
</div>
</template>

<script>
export default {
name: 'Grandson',
inject: [name]
}
</script>
```

这里可以通过inject直接访问其两个层级上的数据，其用法与props完全相同，同样可以参数校验等过去用的 event-bus 虽然可以解决深层问题，但是会导致整个 event-emit 组成过于混乱，难以维护。使用 provide / inject 可以保证父子单向数据流的清晰性。在 React 中 Context 的 Provider / Consumer 也有相同的效果，由于还没有具体使用过，对 React 本身也只有一面之缘，留待以后在了解，感兴趣的同学可以 阅读文档 了解。 当然它的作用还有很多，比如vue 路由参数变化，页面不刷新，provide /inject 完美解决方案

## 四, vuex

状态管理模式、集中式存储管理 一听就很高大上，蛮吓人的。在我看来 vuex 就是把需要共享的变量全部存储在一个对象里面，然后将这个对象放在顶层组件中供其他组件使用。这么说吧，将vue想作是一个js文件、组件是函数，那么vuex就是一个全局变量，只是这个“全局变量”包含了一些特定的规则而已。在vue的组件化开发中，经常会遇到需要将当前组件的状态传递给其他组件。父子组件通信时，我们通常会采用 props + emit 这种方式。但当通信双方不是父子组件甚至压根不存在相关联系，或者一个状态需要共享给多个组件时，就会非常麻烦，数据也会相当难维护，这对我们开发来讲就很不友好。vuex 这个时候就很实用，不过在使用vuex之后也带来了更多的概念和框架，需慎重！

vuex里面都有些什么内容？

```js
const store = new Vuex.Store({
    state: {
        name: 'weish',
        age: 22
    },
    getters: {
        personInfo(state) {
            return `My name is ${state.name}, I am ${state.age}`;
        }
    }
    mutations: {
        SET_AGE(state, age) {
            commit(age, age);
        }
    },
    actions: {
        nameAsyn({commit}) {
            setTimeout(() => {
                commit('SET_AGE', 18);
            }, 1000);
        }
    },
    modules: {
        a: modulesA
    }
}
```

这个就是最基本也是完整的vuex代码；vuex 包含有五个基本的对象：

* state：存储状态。也就是变量
* getters：派生状态。也就是set、get中的get，有两个可选参数：state、getters分别可以获取state中的变量和其他的getters。外部调用方式：store.getters.personInfo\(\)。就和vue的computed差不多
* mutations：提交状态修改。也就是set、get中的set，这是vuex中唯一修改state的方式，但不支持异步操作。第一个参数默认是state。外部调用方式：store.commit\('SET\_AGE', 18\)。和vue中的methods类似。

* actions：和mutations类似。不过actions支持异步操作。第一个参数默认是和store具有相同参数属性的对象。外部调用方式：store.dispatch\('nameAsyn'\)。

* modules：store的子模块，内容就相当于是store的一个实例。调用方式和前面介绍的相似，只是要加上当前子模块名，如：store.a.getters.xxx\(\)。

vue-cli中使用vuex的方式

一般来讲，我们都会采用vue-cli来进行实际的开发，在vue-cli中，开发和调用方式稍微不同

```js
├── index.html
├── main.js
├── components
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── state.js          # 跟级别的 state
    ├── getters.js        # 跟级别的 getter
    ├── mutation-types.js # 根级别的mutations名称（官方推荐mutions方法名使用大写）
    ├── mutations.js      # 根级别的 mutation
    ├── actions.js        # 根级别的 action
    └── modules
        ├── m1.js         # 模块1
        └── m2.js         # 模块2
```

state.js示例：

```js
const state = {
    name: 'weish',
    age: 22
};
export default state;
```

getters.js示例（我们一般使用getters来获取state的状态，而不是直接使用state）：

```js
export const name = (state) => {
    return state.name;
}

export const age = (state) => {
    return state.age
}

export const other = (state) => {
    return `My name is ${state.name}, I am ${state.age}.`;
}
```

mutation-type.js示例（我们会将所有mutations的函数名放在这个文件里）：

```js
export const SET_NAME = 'SET_NAME';
export const SET_AGE = 'SET_AGE';
```

mutations.js示例：

```js
import * as types from './mutation-type.js';

export default {
    [types.SET_NAME](state, name) {
        state.name = name;
    },
    [types.SET_AGE](state, age) {
        state.age = age;
    }
};
```

actions.js示例（异步操作、多个commit时）：

```js
import * as types from './mutation-type.js';

export default {
    nameAsyn({commit}, {age, name}) {
        commit(types.SET_NAME, name);
        commit(types.SET_AGE, age);
    }
};
```

modules--m1.js示例（如果不是很复杂的应用，一般来讲是不会分模块的）：

```js
export default {
    state: {},
    getters: {},
    mutations: {},
    actions: {}
};
```

index.js示例（组装vuex）：

```js
import vue from 'vue';
import vuex from 'vuex';
import state from './state.js';
import * as getters from './getters.js';
import mutations from './mutations.js';
import actions from './actions.js';
import m1 from './modules/m1.js';
import m2 from './modules/m2.js';
import createLogger from 'vuex/dist/logger'; // 修改日志

vue.use(vuex);

const debug = process.env.NODE_ENV !== 'production'; // 开发环境中为true，否则为false

export default new vuex.Store({
    state,
    getters,
    mutations,
    actions,
    modules: {
        m1,
        m2
    },
    plugins: debug ? [createLogger()] : [] // 开发环境下显示vuex的状态修改
});
```

最后将store实例挂载到main.js里面的vue上去就行了

```js
import store from './store/index.js';

new Vue({
  el: '#app',
  store,
  render: h => h(App)
});
```

在vue组件中使用时，我们通常会使用mapGetters、mapActions、mapMutations，然后就可以按照vue调用methods和computed的方式去调用这些变量或函数，示例如下：

```js
import {mapGetters, mapMutations, mapActions} from 'vuex';

/* 只写组件中的script部分 */
export default {
    computed: {
        ...mapGetters([
            'name',
            'age'
        ])
    },
    methods: {
        ...mapMutations({
            setName: 'SET_NAME',
            setAge: 'SET_AGE'
        }),
        ...mapActions([
            nameAsyn
        ])
    }
};
```



