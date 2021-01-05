# 小程序require绝对路径引入

+ 创建时间：2021.1.4

> 微信小程序的require引入文件路径不支持绝对路径，一堆../看着难受，然后想到用一个代理函数来引入

## 例子

+ 定义代理函数

```js
// app.js
App({
    /**
     * @name 绝对路径引入资源
     * @see 注意：调用此方法引入资源，将无法使用“上传时进行代码保护”（只是【上传时】进行保护的话，问题不大）
     * @param { (string) } urls 资源路径列表
     * @returns { Array } 引入的资源列表
     */
    $require: (...urls) => urls.map(url => require(url)),
})
```

+ 使用代理函数

```js
// pages/index/index.js
const [
    $ajax,
    $store,
] = getApp().$require(
    'utils/ajax',
    'utils/store',
) // 使用绝对路径引入，解构赋值接收
Page({
    ...
})
```