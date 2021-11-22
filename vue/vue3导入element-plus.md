导入 `element-ui` 的 `vue3` 支持版本，也就是 [element-plus](http://element-plus.org/#/zh-CN)

`element-plus` 提供了快捷导入的方式，即：[vue-cli-plugin-element-plus](https://github.com/element-plus/vue-cli-plugin-element-plus)，可以通过以下方式来快捷导入 `element-plus` （注意：此种方式会自动修改 `App.vue` 文件）：

1. 在通过 `vue-cli` 创建的项目中，执行

   ```
   vue add element-plus
   ```

2. 选择全局导入

![图片描述](http://szimg.mukewang.com/6163f498097e08cb06860222.png)

3.暂不生成覆盖变量的 `scss` 文件
![图片描述](http://szimg.mukewang.com/6163f4ab0941fdd806770027.png)

4.选择简体中文即可

![图片描述](http://szimg.mukewang.com/6163f4bd098556bb07490164.png)
5. 出现该提示表示安装完成
![图片描述](http://szimg.mukewang.com/6163f4ce09e291fb07150272.png)

6.此时运行项目，则会得到如下错误

![图片描述](http://szimg.mukewang.com/6163f4dc09ec90f512590157.png)

7. 出现该错误的原因是因为 [vue-cli-plugin-element-plus](https://github.com/element-plus/vue-cli-plugin-element-plus) 默认修改了 `APP.vue` 文件，导入了 `HelloWorld`

8.所以需要到 `APP.vue` 中，初始化如下代码：

```vue
<template>
  <router-view />
</template>

<script>
export default {
  name: 'App'
}
</script>

<style></style>
```

那么至此，`element-plus` 导入成功