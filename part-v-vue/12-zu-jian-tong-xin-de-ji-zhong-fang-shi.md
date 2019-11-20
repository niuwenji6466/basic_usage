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



