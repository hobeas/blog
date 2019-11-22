---
title: Node RSA 加密解密
date:  19-9-14 2:54 +08
---

# RSA 加密解密

**流程**
A 向 B 发请求：A 用 A 密钥签名，用 B 公钥加密。
A 获得 B 响应：A 用 B 公钥解密，用 A 密钥验签。

以下是部分实现源码。使用 B 公钥，通过 RSA 分段加密解密。

```js
import NodeRSA from 'node-rsa'
import fs from 'fs'

const PublicPem = fs.readFileSync('./pem/rsa_public_key.pem').toString(),
  PublicKey = new NodeRSA(PublicPem, 'pkcs8-public', { encryptionScheme: 'pkcs1' }),
  ENCRYPT_LIMIT = 117,
  DECRYPT_LIMIT = 172

function encryptRequest(opts) {
  // ... 签名部分省略
  const reqStr = JSON.stringify(opts)
  let encrypted = ''
  for (let i = 0, len = Math.ceil(reqStr.length / ENCRYPT_LIMIT); i < len; i++) {
    const text = reqStr.substr(i * ENCRYPT_LIMIT, ENCRYPT_LIMIT)
    encrypted += PublicKey.encrypt(text, 'base64')
  }
  return encrypted
}

function decryptResponse(resStr) {
  let decrypted = ''
  for (let i = 0, len = Math.ceil(resStr.length / DECRYPT_LIMIT); i < len; i++) {
    const text = resStr.substr(i * DECRYPT_LIMIT, DECRYPT_LIMIT)
    decrypted += PublicKey.decryptPublic(text, 'utf8')
  }
  return JSON.parse(decrypted)
}
```

# 踩坑

**问题**
我们公司（即 A）与 B 公司（即 B）API 对接时，使用 B 提供的公钥进行加密，B 却解密不了。

**原因**
A 用 node，B 用 java。node 默认的 encryptionScheme 是 pkcs1_oaep，而 java 默认是 pkcs1。

**解决**
找到原因就好办了，生成公钥对象时，将 encryptionScheme 配置为 pkcs1 即可。


# 其他

生成 RSA 公钥和私钥

```sh
# 生成1024位的私钥
openssl genrsa -out rsa_private_key.pem 1024
# 生成公钥
openssl rsa -in rsa_private_key.pem -out rsa_public_key.pem -pubout
# 把私钥转换成 PKCS8 格式
# openssl pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM -nocrypt
```


参考资料：
- [node-rsa npm](https://www.npmjs.com/package/node-rsa)
- [RSA加密解密坑](https://blog.csdn.net/mshootingstar/article/details/56496719)
- [mac上使用生成RSA公钥和密钥](https://www.cnblogs.com/kris888/p/3875495.html)
