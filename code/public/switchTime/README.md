# 转换时间格式

+ 创建时间：2021.1.5

> 转换时间为不同的格式

## 例子

+ 定义函数

```js
/**
 * @name 转换时间格式
 * @param { number | string | boolean } [ time = new Date() ] 要转换的时间戳（传 false 使用默认值）
 * @param { string } [ format = "****-**-**" ] 指定转换格式（必须用标识符中间隔开）
 * @param { string } [ fo = "*" ] 指定转换格式的标识符
 * @returns { string } 转换之后的时间
 */
const switchTime = (time, format = '****-**-**', fo = '*') => {
    let data = time ? new Date(time * 1 || time.replace(/-/g, '/')) : new Date(), index = 2
    let y = data.getFullYear()
    let time_ = format[index] == fo ?
        format.replace(fo + fo + fo + fo, y) : // ****年
        format.replace(fo + fo, `${y}`.substring(2)) // **年
    index = time_.indexOf(fo)
    if (index == -1) return time_
    const fillTime = val => { // 填入时间
        time_ = time_[index + 1] == fo ?
            time_.replace(fo + fo, `${val}`.padStart(2, 0)) : // **
            time_.replace(fo, val) // *
        index = time_.indexOf(fo)
        return index == -1
    }
    if (fillTime(data.getMonth() + 1)) return time_ // 月
    if (fillTime(data.getDate())) return time_ // 日
    if (fillTime(data.getHours())) return time_ // 时
    if (fillTime(data.getMinutes())) return time_ // 分
    fillTime(data.getSeconds()) // 秒
    return time_
}
```

+ 使用函数

```js
console.log(
    switchTime(), // 2021-01-05
    switchTime(Date.now() - 24*60*60*1000), // 2021-01-04
    switchTime('2021-01-05 11:11:11'), // 2021-01-05
)
console.log(
    switchTime(null, '****-**-** **:**:**'), // 2021-01-05 11:11:11
    switchTime(null, '****年**月**日'), // 2021年01月05日
    switchTime(null, '**年*月*日 *时*分*秒'), // 21年1月5日 11时11分11秒
    switchTime(null, '当前时间：** 年 * 月 * 日   * 时'), // 当前时间：21 年 1 月 5 日   11 时
)
console.log(
    switchTime(null, '####年##月##日', '#'), // 2021年01月05日
)
```