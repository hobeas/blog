---
title: ts 踩坑：tsconfig.json 路径别名无效
date:  20-3-29 15:50 +08
---

# tsconfig.json 路径别名无效

在 src/bin/www.ts 文件里导入 src/config/index.ts

```ts
import config from '@/config'
```

预想编译结果

```ts
var config_1 = __importDefault(require("../config"));
```

实际编译结果

```ts
var config_1 = __importDefault(require("@/config"));
```

已在 tsconfig.json 设置了

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

尝试过的办法

1. 用 vscode 自带的功能监听编译 ts
  终端 => 运行任务 => "tsc:监视 - tsconfig.json"（无效）

2. 安装 typescript 模块，使用 tsc 命令（无效）
  `npm i -D typescript`
  `tsc`

# 解决办法

用 ttypescript 模块导入转换路径的插件

```sh
npm i -D ttypescript typescript-transform-paths
```

package.json 更改

```json
{
  "scripts": {
    "watch": "ttsc -w",
    "build": "ttsc"
  }
}
```

tsconfig.json 添加

```json
{
  "compilerOptions": {
    "plugins": [
      { "transform": "typescript-transform-paths" }
    ]
  }
}
```

现在监听不用 vscode 自带的功能，而是用 `npm run watch`

打包还是 `npm run build`，只不过把 `tsc` 改成 `ttsc`，改变不大，解决！
