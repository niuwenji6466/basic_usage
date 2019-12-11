# vue中select控件使用

```html
<FormItem prop="type" label="企业类别">
  <Select v-model="form.type" style="width:400px">
    <Option v-for="item in typeList" :value="item.value" :key="item.value">{{ item.label }}</Option>
  </Select>
</FormItem>
```

```js
created:function(){
    this.form.type= this.typeList[0].value;
}
```

```
data() {
    return {
      form: {
        userName: "",
        password: "",
        repassword: "",
        companyTitle: "",
        address: "",
        telephone: "",
        areaCode: "",
        areaName: "",
        type:"",
        random: ""
      },
      typeList: [
          { value: "DA",label: "日常安全"},
          { value: "CE",label: "继续教育"}
      ],
      areaId: "",
      area: ["辽宁省", "大连市", "中山区"],
      randomUrl: this.$config.randomUrl + "/random"
    };
  },
```



