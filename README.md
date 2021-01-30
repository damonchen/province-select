# province-select
province select using by uni-app

- 使用省、市、区三级的选择

## 使用

将js代码和prefectures-picker.vue复制到uni-app对应的组件目录下

```
    <prefectures-picker sep="-" @change="onChange">
    </prefectures-picker>
```

- sep：表示省、市、区之间的分割线
- change事件在内部选择后抛出来，其事件由以下三个字段组成:
  - province
  - city
  - prefecture

上述每个字段，结构均为：

```
{
  "code": xxxx  // 对应的编码
  "name": xxxx  // 名称
}
```

