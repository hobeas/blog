---
title: Node RSA 加密解密（二）
date:  19-11-12 0:40 +08
---

**前言**
这篇相对前一篇，加密规则略有不同，遇到很多坑，因此记录下备忘

# 加密解密

**流程**
A 向 B 发请求：A 用 A 密钥签名，用 B 公钥加密。
A 获得 B 响应：A 用 A 私钥解密，用 A 密钥验签。

> 这次双方交换了公钥（2048位），都用对方公钥加密，用己方私钥解密。

**加密规则**
分段加密为 byte 数组（坑），合并后整体 base64。

**加签规则**
> 因为遇到坑，所以也记录下加签规则。

除加密字段（即下文 content 和 sign）外，所有字段按键名 ASCLL 码从小到大排序，使用键值对格式（即 `k1=v1&k2=v2`）拼接成字符串，加上密钥，指定字符集GBK（坑），进行 `md5`。


以下是部分实现源码。

```js
import NodeRSA from 'node-rsa'
import fs from 'fs'
import encoding from 'encoding'
import md5 from 'md5'

const BPublicKey = new NodeRSA(fs.readFileSync('./pem/b_public_key.pem').toString(), 'pkcs8-public', { encryptionScheme: 'pkcs1' }),
  MyPrivateKey = new NodeRSA(fs.readFileSync('./pem/my_private_key.pem').toString(), 'pkcs8-private', { encryptionScheme: 'pkcs1' }),
  ENCRYPT_LIMIT = 245,
  // 这里还可能为 256、344、684、1024
  DECRYPT_LIMIT = 512,
  KEY = '123'

function signature(body) {
  const str = Object.keys(body)
    .filter(k => k !== 'content' && k !== 'sign')
    .sort()
    .map(k => `${k}=${(body[k] !== null && body[k] !== undefined) ? body[k] : ''}`)
    .join('&') + KEY
  // 是的，为了 utf8 转 GBK，引入一个库
  return md5(encoding.convert(str, 'GBK'))
}

function encryptContent(str) {
  let buffList = Buffer.alloc(0)
  for (let i = 0, len = Math.ceil(str.length / ENCRYPT_LIMIT); i < len; i++) {
    const text = str.substr(i * ENCRYPT_LIMIT, ENCRYPT_LIMIT)
    const buff = BPublicKey.encrypt(text)
    buffList = Buffer.concat([buffList, buff])
  }
  return Buffer.from(buffList).toString('base64')
}

function decryptContent(str) {
  const buffList = Buffer.from(str, 'base64')
  let decrypted = ''
  for (let i = 0, len = Math.ceil(buffList.length / DECRYPT_LIMIT); i < len; i++) {
    const buff = buffList.slice(i * DECRYPT_LIMIT, i * DECRYPT_LIMIT + DECRYPT_LIMIT)
    decrypted += MyPrivateKey.decrypt(buff, 'utf8')
  }
  return decrypted
}

function obj2str(body, isEncode) {
  if (!isEncode)
    return Object.keys(body).map(k => `${k}=${body[k]}`).join('&')
  return Object.keys(body).map(k => `${k}=${encodeURIComponent(body[k])}`).join('&')
}

function str2obj(str, isDecode) {
  const obj = {}
  str.split('&').map(item => {
    const hash = item.split('=')
    const val = hash[1] || ''
    obj[hash[0]] = isDecode ? decodeURIComponent(val) : val
  })
  return obj
}

// 加签、加密
function handleRequest() {
  const body = {
    k1: 'v1',
    content: ''
  }
  const opts = {
    k2: 'v2',
    sign: ''
  }
  opts.sign = signature({ ...body, ...opts })
  body.content = encryptContent(obj2str(opts))
  console.log('requested params', obj2str(body, 'encode'))
}

// 解密、验签
function handleResponse() {
  // bodyStr 为上一步的 console
  const bodyStr = ''
  let body = str2obj(bodyStr, 'decode')
  const opts = str2obj(decryptContent(body.content))
  body = { ...body, ...opts }
  const sign = signature(body)
  console.log('body', body, 'verify sign', sign === opts.sign)
}

// handleRequest()
// handleResponse()
```

# 踩坑

**加解密长度问题**

对于加密明文大小，因为之前了解过，就想当然写了117（128减11），但那是对于1024位的，这次是2048位的，为245（256减11）。

对于解密密文大小，还没完全搞清楚，网上说是256，实践证明并不是。初步认为跟要加密的明文有关，256、344、512、684、1024，测试时这些值都出现过，最后写了较稳定的512，存在潜在的bug，之后再探索。

**UTF8 转 GBK**

以为只是简单格式转换，搜了一圈 JS 并没有这样的 API。那 md5 模块应该有这样的 encoding 配置，查了文档，并没有。那应该有人写了这样的方法，试了几个根本不能用。直到最后发现 `encoding` 这个库，总算一行解决了。

**分段加密**

`node-rsa` 是可以直接加密为 base64 的，网上也几乎是这样的用法，即
```js
PublicKey.encrypt(text, 'base64')
```
但对方 java 系统是分段加密为 byte 数组，最后转 base64。

他分段加密的结果到底是什么？于是我在 `node-rsa` 的 `encrypt()` 方法的第二个参数纠结，最简单粗暴的方法，一个个试过去咯，什么 ascii、utf8、hex、binary，试了个遍都没用。后来看到篇文章加密为 buffer，我就猜，这个 byte 数组可能对应 buffer，毕竟 buffer 跟数组很像。抱着试试看的心态，看起 buffer 的文档，alloc、concat、copy 这些不常用的方法都被我整到一起，所幸终于解决了！
