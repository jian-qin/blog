# 小程序自定义全局变量

+ 创建时间：2021.1.5

> 自定义全局变量，用来存储一些临时状态

## 例子

+ 定义存储对象

```js
// utils/store.js
const $store = {}
/**
 * @name 内部函数-重置数据
 * @param { object } [ ignore ] 白名单
 * @returns { Object } 重置之后的数据
 */
const $reset = ignore => Object.assign(
    $store, // 运行中数据
    { // 初始数据
        $reset, // 内部函数-重置数据

        /* ---- 模拟数据 S ---- */
        token: null, // 秘钥
        cache: null, // 缓存页面传参
        uuid: null, // uuid
        authentication: false, // 认证状态
        userName: null, // 用户名
        phone: null, // 手机号
        userType: 1, // 用户类型
        userId: null, // 用户id
        /* ---- 模拟数据 E ---- */

    },
    ignore // 重置数据-白名单
    || { // 重置数据-默认白名单

        uuid: $store.uuid /* ---- 模拟数据 ---- */

    }
)
$reset({})
module.exports = $store
```

+ 调用存储数据

```js
// pages/index/index.js
const $store = require('../../utils/store.js')
// 这里的引入有更好的方法：（小程序require绝对路径引入）<code/applet/pathAgent/README.md>

$store.token = '00000000' // 存
console.log($store.token) // 取

$store.$reset() // 重置-使用默认白名单
$store.$reset({ // 重置-自定义白名单
     phone: $store.phone, // 使用‘老数据’，实现白名单效果
     password: 123456, // 当然也可以直接赋值，等同于：‘重置’然后‘存’
})
```