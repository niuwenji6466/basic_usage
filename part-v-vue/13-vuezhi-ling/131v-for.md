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



