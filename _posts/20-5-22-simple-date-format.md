---
title: 简单的时间格式化
date: 20-5-22 00:53 +08
---

时间格式化是很常见的需求，即便是微信开发者工具初始化的极简项目里，`util.js` 也写了一个公用方法 `formatTime()`，其格式是固定的。

如果需要灵活的格式化，[moment](https://momentjs.com) 是不错的选择，但小程序单包（主包/分包）大小不能超过 2M，这限制了我们不能导入太多功能库。当然，另一方面也提醒我们精简代码。

如果只是为了显示，不操作日期，以下方法可满足一般的需求，直接上代码

```js
/**
 * @param {String} fmt eg: yyyy-MM-dd hh:mm
 * @author meizz
 * https://blog.csdn.net/meizz/article/details/405708
 */
Date.prototype.Format = function (fmt) {
  const o = {
    'M+': this.getMonth() + 1,
    'd+': this.getDate(),
    'h+': this.getHours(),
    'm+': this.getMinutes(),
    's+': this.getSeconds(),
    'q+': Math.floor((this.getMonth() + 3) / 3),
    S: this.getMilliseconds()
  }
  if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + '').substr(4 - RegExp.$1.length))
  for (let k in o) if (new RegExp('(' + k + ')').test(fmt)) fmt = fmt.replace(RegExp.$1, RegExp.$1.length == 1 ? o[k] : ('00' + o[k]).substr((o[k] + '').length))
  return fmt
}
```

这个方法来自 [meizz](https://blog.csdn.net/meizz/article/details/405708)，以下是简单的几个栗子

```js
// 最常用的格式
new Date().Format('yyyy-MM-dd hh:mm') // 2020-05-20 13:14

const log = console.log
const date = new Date('2020-05-20 13:14:20:233')

log(date.Format('yyyy-MM-dd')) // 日期：2020-05-20
log(date.Format('yy-MM-dd')) // 年份显示后两位：20-05-20
log(date.Format('yy-M-d')) // 个位数时不补0：20-5-20
log(date.Format('yyyy年M月d日')) // 自定义分隔符：2020年5月20日
log(date.Format('M/d/yyyy')) // 自定义顺序：5/20/2020

log(date.Format('h:m:s')) // 时间：13:14:20
log(date.Format('hh:mm:ss S')) // 毫秒：13:14:20 233

// 随便你写，有出现 y M d h m s q S 字符的都可以
log(date.Format('现在是yyyy年M月份，第q季度')) // 现在是2020年5月份，第2季度
```

`Format()` 方法是为了处理格式化显示，如果需要对时间进行一丢丢加减操作呢（如使用 localStorage 缓存数据常需要设置过期时间），于是我写了个方法

```js
/**
 * @param {Number} num
 * @param {String} unit enum: y,M,d,h,m,s
 */
Date.prototype.Add = function (num, unit) {
  const s = 1000, m = s * 60, h = m * 60, d = h * 24, M = d * 30, y = M * 12
  const obj = { s, m, h, d, M, y }
  if (!Object.keys(obj).includes(unit)) throw new Error(`Invalid param unit`)
  return new Date(this.getTime() + num * obj[unit])
}
```

简单使用的栗子🌰

```js
const log = console.log
const date = new Date('2020-05-20 13:14:20:233')

// 加两小时
log(date.Add(2, 'h')) // 2020-05-20T07:14:20.233Z

// 减 10 分钟
log(date.Add(-10, 'm')) // 2020-05-20T05:04:20.233Z

// 返回为 Date 对象，可配合 Format() 链式调用
log(date.Add(1, 'd').Format('明天是M月d日')) // 明天是5月21日
```
