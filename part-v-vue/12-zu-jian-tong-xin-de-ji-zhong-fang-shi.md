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

