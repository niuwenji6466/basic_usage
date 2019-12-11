# vue中select控件使用



```html
<FormItem prop="type" label="企业类别">
  <Select v-model="form.type" style="width:400px">
    <Option v-for="item in typeList" :value="item.value" :key="item.value">{{ item.label }}</Option>
  </Select>
</FormItem>
```



