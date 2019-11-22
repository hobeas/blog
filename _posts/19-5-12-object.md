---
title: Object 的一些属性
date:  19-5-12 14:31 +08
tags:  js
---

方法 | 说明
-|-
preventExtensions | 阻止扩展。不可添加属性
isExtensible | 判断扩展
seal | 封闭对象。不可添加属性，不可删除
isSealed | 判断密封
freeze | 冻结对象。不可添加属性，不可删除，不可写
isFrozen | 判断冻结
defineProperty | 定义属性

### 扩展：`preventExtensions` 和 `isExtensible`
```js
// 常量对象不能重新赋值，但能添加属性
const obj = { a: 1 }
// 禁用其添加属性
Object.preventExtensions(obj)
Object.isExtensible(obj) // false
obj.b = 2
console.log(obj) // { a: 1 }
// 但可更改，可删除既有属性
obj.a = 2 // { a: 2 }
delete obj.a // {}
```

### 密封：`seal` 和 `isSealed`
```js
const obj = { a: 1 }
Object.seal(obj)
Object.isSealed(obj) // true
// 可更改，不可删除
obj.a = 2 // { a: 2 }
delete obj.a // { a: 2 }

// 把不可扩展对象的全部属性变得不可配置，即密封对象
Object.preventExtensions(obj)
Object.defineProperty(obj, 'a', { configurable: false })
```

### 冻结：`freeze` 和 `isFrozen`
```js
const obj = { a: 1 }
Object.freeze(obj)
// 不可更改，不可删除
obj.a = 2 // { a: 1 }
delete obj.a // { a: 1 }

// 把密封对象的全部属性变得不可写，即冻结对象
Object.seal(obj)
Object.defineProperty(obj, 'a', { writable: false })

// 只是浅冻结
const obj = { a: { b: 1 } }
Object.freeze(obj)
Object.isFrozen(obj.a) // false
obj.a.b = 2
console.log(obj.a.b) // 2

// 深冻结属性
Object.prototype.deepFreeze = obj => {
  Object.freeze(obj)
  Object.values(obj).forEach(val => {
    if (typeof val === 'object')
      deepFreeze(val)
  })
}
Object.deepFreeze(obj)
obj.a.b = 2
console.log(obj.a.b) // 1
```