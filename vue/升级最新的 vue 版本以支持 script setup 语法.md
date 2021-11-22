因为使用 `@vue/cli-v4.5.13` 版本创建的项目中，`vue` 的版本为 `v 3.0.0` 。

如果需要使用最新的 `script setup 语法`，该语法在 `v 3.0.0` 版本中是不支持的，所以需要升级 `vue` 版本。

执行：

```js
npm i vue@3.2.8 vue-router@4.0.11 vuex@4.0.2
```

升级之后，查看 `package.json` 得到的版本应为：

```json
"vue": "^3.2.8",
"vue-router": "^4.0.11",
"vuex": "^4.0.2"
```