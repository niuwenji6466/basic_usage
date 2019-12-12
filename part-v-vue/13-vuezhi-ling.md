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



