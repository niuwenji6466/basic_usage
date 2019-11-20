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

* 总结：



