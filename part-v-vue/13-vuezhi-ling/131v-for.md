# Vue指令之 v-for的使用Vue指令

# 1.迭代普通数组

* 在data中定义普通数据

```
data:{
      list:[1,2,3,4,5,6]
}
```

* 在html中使用 v-for 指令渲染

```
<p v-for="(item,i) in list">--索引值--{{i}}   --每一项--{{item}}</p>
```

# 2.迭代对象数组

* 在data中定义对象数组

```
data:{
      list:[1,2,3,4,5,6],
      listObj:[
        {id:1, name:'zs1'},
        {id:2, name:'zs2'},
        {id:3, name:'zs3'},
        {id:4, name:'zs4'},
        {id:5, name:'zs5'},
        {id:6, name:'zs6'},
      ]
}
```

* 在html中使用 v-for 指令渲染

```
<p v-for="(user,i) in listObj">--id--{{user.id}}   --姓名--{{user.name}}</p>
```

# 3.迭代对象

* 在data中定义对象

```
data:{
      user:{
        id:1,
        name:'托尼.贾',
        gender:'男'
      }
}
```

* 在html中使用 v-for 指令渲染

```
<p v-for="(val,key) in user">--键是--{{key}}--值是--{{val}}</p>
```

# 4.迭代数字

```
<!-- 注意：如果使用v-for迭代数字的话，前面 count 的值从 1 开始-->
<p v-for="count in 10">这是第{{count}}次循环</p>
```

完整代码：

```html
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  </head>
<body>
  <div id='app'>
    <!--v-for循环普通数组-->
    <p v-for="(item,i) in list">--索引值--{{i}}   --每一项--{{item}}</p>
    <br/>
    <!--v-for循环对象数组-->
    <p v-for="(user,i) in listObj">--id--{{user.id}}   --姓名--{{user.name}}</p>
    <br/>
    <!--注意，在遍历对象的键值对的时候，除了有 val 和 key,在第三个位置还有一个索引-->
    <p v-for="(val,key) in user">--键是--{{key}}  --值是--{{val}}</p>
    <br/>
    <!-- in 后面我们放过数组、对象数组、对象，还可以放数字-->
    <!-- 注意：如果使用v-for迭代数字的话，前面 count 的值从 1 开始-->
    <p v-for="count in 10">这是第{{count}}次循环</p>
  </div>
</body>
<script src="vue.min.js"></script>
<script>
  var vm = new Vue({
    el:'#app',
    data:{
      list:[1,2,3,4,5,6],
      listObj:[
        {id:1, name:'zs1'},
        {id:2, name:'zs2'},
        {id:3, name:'zs3'},
        {id:4, name:'zs4'},
        {id:5, name:'zs5'},
        {id:6, name:'zs6'},
      ],
      user:{
        id:1,
        name:'托尼.贾',
        gender:'男'
      }
    }
  });
</script>
</html>
```



