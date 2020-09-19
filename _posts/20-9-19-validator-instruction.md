---
title: validator instruction
date: 20-9-19 17:22 +08
---

# [validator](https://github.com/validatorjs/validator.js)


## Validators

校验方法                                 | 描述
--------------------------------------- | --------------------------------------
**contains(str, seed [, options ])**    | 包含指定字符<br/>`options` 默认 `{ ignoreCase: false}` 大小写敏感
**equals(str, comparison)**             | 相等
**isAfter(str [, date])**               | 在指定日期之后（默认现在）
**isAlpha(str [, locale])**             | a-zA-Z
**isAlphanumeric(str [, locale])**      | a-zA-Z0-9
**isAscii(str)**                        | ASCII
**isBase32(str)**                       | base32
**isBase64(str [, options])**           | base64<br/>`options` 默认 `{urlSafe: false}`. [url safe](https://base64.guru/standards/base64url)
**isBefore(str [, date])**              | 在指定日期之前
**isBIC(str)**                          | 银行识别码 (Bank Identification Code) 或 银行国际代码 (SWIFT Code)
**isBoolean(str)**                      | boolean
**isBtcAddress(str)**                   | 比特币 BTC 地址
**isByteLength(str [, options])**       | 长度范围<br/>`options` 默认 `{min:0, max: undefined}`
**isCreditCard(str)**                   | 信用卡
**isCurrency(str [, options])**         | 货币金额。[options](#refer-is-currency-options)
**isDataURI(str)**                      | [data uri 格式](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs)
**isDate(input [, format])**            | 日期<br/>`format` 默认 `YYYY/MM/DD`
**isDecimal(str [, options])**          | 小数<br/>`options` 默认 `{force_decimal: false, decimal_digits: '1,', locale: 'en-US'}`。`decimal_digits` 最小1位 `1,`，固定3位 `3`，指定范围 `1,3`
**isDivisibleBy(str, number)**          | 整除
**isEAN(str)**                          | 欧洲商品编码 (European Article Number)
**isEmail(str [, options])**            | 邮箱。[options](#refer-is-email-options)
**isEmpty(str [, options])**            | 长度为0<br/>`options` 默认 `{ ignore_whitespace:false }`
**isEthereumAddress(str)**              | [以太坊](https://ethereum.org/) 地址
**isFloat(str [, options])**            | 浮点数<br/>`options` 字段 `min`, `max`, `gt`, `lt`
**isFQDN(str [, options])**             | 全域名 (Fully Qualified Domain Name)<br/>`options` 默认 `{ require_tld: true, allow_underscores: false, allow_trailing_dot: false }`
**isFullWidth(str)**                    | 全角字符
**isHalfWidth(str)**                    | 半角字符
**isHash(str, algorithm)**              | 哈希值
**isHexadecimal(str)**                  | 16进制数字
**isHexColor(str)**                     | 16进制颜色
**isHSL(str)**                          | Hue 色相、Saturation 饱和度、Lightness 亮度、Alpha 透明度。[CSS颜色4级规范](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)，支持空格分隔(少数除外如: `hsl(200grad+.1%62%/1)`)
**isIBAN(str)**                         | 国际银行账户 (International Bank Account Number)
**isIdentityCard(str [, locale])**      | 身份证号
**isIMEI(str [, options]))**            | IMEI 格式 `###############` 或 `##-######-######-#`<br/>默认第一种格式，若 `options` 设字段 `{allow_hyphens: true}` 则验证第二种格式
**isIn(str, values)**                   | 在允许值数组中
**isInt(str [, options])**              | 整数<br/>`options` 字段 `min`, `max`, `gt`, `lt`, `allow_leading_zeroes`。设 `{allow_leading_zeroes: false}` 时不允许 0 开头
**isIP(str [, version])**               | IP (v4 或 v6)
**isIPRange(str)**                      | IPv4
**isISBN(str [, version])**             | 国际标准书号 (International Standard Book Number) v10 或 v13
**isISIN(str)**                         | [国际证券识别码](https://en.wikipedia.org/wiki/International_Securities_Identification_Number) (International Securities Identification Number)（股票/证券号）
**isISO8601(str)**                      | 日期 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)<br/>设置 `options.strict = true`时 `2009-02-29` 无效
**isISO31661Alpha2(str)**               | 国家代码 [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)
**isISO31661Alpha3(str)**               | 国家代码 [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3)
**isISRC(str)**                         | 国际标准音像制品码 [ISRC](https://en.wikipedia.org/wiki/International_Standard_Recording_Code)
**isISSN(str [, options])**             | 国际标准刊号 [ISSN](https://en.wikipedia.org/wiki/International_Standard_Serial_Number)<br/>`options` 默认 `{ case_sensitive: false, require_hyphen: false }`。`case_sensitive` 为 true 时小写 `'x'` 不能作为校验位
**isJSON(str [, options])**             | JSON<br/>`options` 默认 `{ allow_primitives: false }`，为 true 时，`true`、`false`、`null` 都为有效值
**isJWT(str)**                          | JWT。格式 `[HEAD].[PAYLOAD].[SIGN]`
**isLatLong(str [, options])**          | 经纬度 (latitude longitude)。格式 `lat,long`<br/>`options` 默认 `{ checkDMS: false }`。设 `true` 验证 度分秒(degrees, minutes, seconds)
**isLength(str [, options])**           | 长度范围<br/>`options` 默认 `{min:0, max: undefined}`
**isLocale(str)**                       | 区域
**isLowercase(str)**                    | 小写
**isMACAddress(str)**                   | MAC 地址<br/>`options` 默认 `{no_colons: false}`。设 `true` 不允许带冒号、连字符、空格、点
**isMagnetURI(str)**                    | [磁链](https://en.wikipedia.org/wiki/Magnet_URI_scheme)
**isMD5(str)**                          | MD5<br/>等同 `isHash(str, 'md5')`
**isMimeType(str)**                     | 文件类型 [MIME](https://en.wikipedia.org/wiki/Media_type)
**isMobilePhone(str [, locale [, options]])** | 手机号<br/>`locale` 默认 `'any'`<br/>若`options` 设 `strictMode` 为 `true` 则必须 `+` 开头
**isMongoId(str)**                      | [MongoDB ObjectId](http://docs.mongodb.org/manual/reference/object-id/)
**isMultibyte(str)**                    | 多字节
**isNumeric(str [, options])**          | 数字<br/>`options` 默认 `{no_symbols: false}`，设 `true` 不允许带符号 (如 `+`, `-`, `.`)
**isOctal(str)**                        | 8进制
**isPassportNumber(str, countryCode)**  | 护照编号
**isPort(str)**                         | 端口号
**isPostalCode(str, locale)**           | 邮政编码
**isRFC3339(str)**                      | 日期 [RFC 3339](https://tools.ietf.org/html/rfc3339)
**isRgbColor(str [, includePercentValues])** | 颜色 rgb 或 rgba<br/>`includePercentValues` 默认 `true`，设 `false` 不允许百分比 `%` 的值
**isSemVer(str)**                       | [语义化版本](https://semver.org/) (Semantic Versioning Specification)<br/>形如 `[主版本].[次版本].[修订]`
**isSurrogatePair(str)**                | 代理对
**isUppercase(str)**                    | 大写
**isSlug**                              | slug<br/>把字符串中所有空格替换为 `-`，字符串转小写，即为一个 slug，用于 URI
**isTaxID(str, locale)**                | 税务识别号 (Tax Identification Number)<br/>`locale` 默认 `en-US`
**isURL(str [, options])**              | URL。[options](#refer-is-url-options)
**isUUID(str [, version])**             | UUID (版本 3, 4, 5)
**isVariableWidth(str)**                | 全角半角字符
**isWhitelisted(str, chars)**           | 白名单
**matches(str, pattern [, modifiers])** | 正则匹配

## Sanitizers

以下方法会改变原数据

消除方法                                | 描述
-------------------------------------- | -------------------------------
**blacklist(input, chars)**            | 删除在黑名单的字符
**escape(input)**                      | 用 HTML 实体替换 `<`, `>`, `&`, `'`, `"`, `/`
**ltrim(input [, chars])**             | 去掉左侧空格
**normalizeEmail(email [, options])**  | 规范化邮箱。[options](#refer-normalize-email-options)
**rtrim(input [, chars])**             | 去掉右侧空格
**stripLow(input [, keep_new_lines])** | 删除 < 32 和 127 的字符
**toBoolean(input [, strict])**        | 转 `boolean` 型。除 `'0'`, `'false'`, `''` 外全为 `true`，严格模式下只有 `'1'` 和 `'true'` 为 `true`
**toDate(input)**                      | 转 日期 或 `null`
**toFloat(input)**                     | 转 浮点数 或 `NaN`
**toInt(input [, radix])**             | 转 整数 或 `NaN`
**trim(input [, chars])**              | 去掉前后空格
**unescape(input)**                    | 用 `<`, `>`, `&`, `'`, `"`, `/` 替换 HTML 实体
**whitelist(input, chars)**            | 删除不在白名单的字符


## 附

<div id="refer-is-currency-options">isCurrency options 默认值</div>

```js
options = {
  symbol: '$',
  require_symbol: false,
  allow_space_after_symbol: false,
  symbol_after_digits: false,
  allow_negatives: true,
  parens_for_negatives: false,
  negative_sign_before_digits: false,
  negative_sign_after_digits: false,
  allow_negative_sign_placeholder: false,
  thousands_separator: ',',
  decimal_separator: '.',
  allow_decimal: true,
  require_decimal: false,
  digits_after_decimal: [2], // 使用确切位数而不是范围，如范围1到3应写为 [1,2,3]
  allow_space_after_digits: false
}
```

<div id="refer-is-email-options">isEmail options 默认值</div>

```js
options = {
  allow_display_name: false, // true 允许 'Display Name <email-address>'
  require_display_name: false, // true 必须符合 'Display Name <email-address>'
  allow_utf8_local_part: true, // false 必须为英文 UTF8 字符
  require_tld: true, // false 允许非顶级域名
  // ignore_max_length: false, // true 忽略标准最大长度
  allow_ip_domain: false, // true 允许 IP 地址
  domain_specific_validation: false // true 启用额外限制。如某些格式有效但被 GMail 拒绝的邮箱
}
```

<div id="refer-is-url-options">isURL options 默认值</div>

```js
options = {
  protocols: ['http', 'https', 'ftp'], // 可修改有效协议
  require_tld: true,
  require_protocol: false, // true 必须包含协议
  require_host: true, // false 不检查主机
  require_valid_protocol: true,
  allow_underscores: false,
  host_whitelist: false,
  host_blacklist: false,
  allow_trailing_dot: false,
  allow_protocol_relative_urls: false,
  disallow_auth: false
  // validate_length: true, // false 不验证长度（IE 限制长度 2083）
}
```

<div id="refer-normalize-email-options">normalizeEmail options 默认值</div>

```js
options = {
  all_lowercase: true, // 将 @ 之前的字符转小写
  gmail_lowercase: true, // gmail 小写（即便 all_lowercase 为 false）
  gmail_remove_dots: true, // 移除点（'.'）
  gmail_remove_subaddress: true, // 移除子地址（如 '+foo'）
  gmail_convert_googlemaildotcom: true, // '@googlemail.com' 转 'gmail.com'
  outlookdotcom_lowercase: true,
  outlookdotcom_remove_subaddress: true,
  yahoo_lowercase: true,
  yahoo_remove_subaddress: true,
  yandex_lowercase: true,
  icloud_lowercase: true,
  icloud_remove_subaddress: true
}
```


