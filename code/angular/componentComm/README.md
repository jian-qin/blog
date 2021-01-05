# angular组件通讯

+ 创建时间：2021.1.5

> angular组件间的通讯方式

+ 在示例中将省略定义数据类型

## 父 -> 子（传参）

```html
<!-- 父组件 -->
<app-child [ispop]='isPop'/>
```

```ts
// 子组件
import { Input } from '@angular/core'

@Input() ispop = false // 接收传参
```

## 子 -> 父（自定义事件）

```ts
// 子组件
import { Output, EventEmitter } from '@angular/core'

@Output() popfn = new EventEmitter() // 定义 自定义事件

...
this.popfn.emit('传参') // 调用 自定义事件
```

```html
<!-- 父组件 -->
<app-child (popfn)='isPop = $event'/>
```

## 父 <-> 子（双向数据绑定）

```html
<!-- 父组件 -->
<app-child [(ispop)]='isPop'/>
```

```ts
// 子组件
import { Input, Output, EventEmitter } from '@angular/core'

@Input() ispop = false // 接收传参
@Output() ispopChange = new EventEmitter() // 定义 双向数据绑定事件
// 双向数据绑定事件 和 普通自定义事件 的区别在事件名称上：{传参名} + 'Change'

...
this.ispopChange.emit(true) // 调用 双向数据绑定事件
```

## 父 <--> 子（双向数据绑定-监听传参变化）

```html
<!-- 父组件 -->
<app-child [ispop]='isPop' (ispopChange)='isPop = $event'/>
```

```ts
// 子组件
import { Input, Output, EventEmitter, OnChanges } from '@angular/core'

@Input() ispop = false // 接收传参
@Output() ispopChange = new EventEmitter() // 定义 双向数据绑定事件

ngOnChanges(changes) {
    if (changes.ispop.currentValue === true) {
        console.log('ispop被修改为true了')
    }
}

...
this.ispopChange.emit(true) // 调用 双向数据绑定事件
```