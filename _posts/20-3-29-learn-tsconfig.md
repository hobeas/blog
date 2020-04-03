---
title: tsconfig.json 配置项说明
date:  20-3-29 14:23 +08
---

```json
{
  "compilerOptions": {
    /* Basic Options */
    "incremental": true,                   // 启用增量编译
    "target": "es5",                       // 指定编译后的目标版本: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 'ESNEXT'
    "module": "commonjs",                  // 指定要使用的模块标准: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', 'ESNext'
    // "lib": [],                             // 指定要包含在编译中的库文件。例：["es6", "dom"]
    "allowJs": true,                       // 允许编译 js 文件
    "checkJs": true,                       // 检查和报告 js 文件中的错误
    // "jsx": "preserve",                     // 指定 jsx 代码用于的开发环境: 'preserve', 'react-native', 'react'
    // "declaration": true,                   // 生成相应的 '.d.ts' 声明文件。但 declaration 和 allowJs 不能同时设为 true
    // "declarationMap": true,                // 为 .d.ts 生成 map 文件
    // "sourceMap": true,                     // 生成相应的 .map 文件
    // "outFile": "./",                       // 将输出合并为一个文件。只有设置 module 值为 amd 或 system 模块时才支持这个配置
    // "outDir": "./",                        // 输出文件夹
    // "rootDir": "./",                       // 编译文件的根目录
    // "composite": true,                     // 启用项目编译
    // "tsBuildInfoFile": "./",               // 指定存储增量编译信息的文件。默认: 'tsconfig.tsbuildinfo'
    "removeComments": true,                // 移除注释
    // "noEmit": true,                        // 不生成编译文件
    // "importHelpers": true,                 // 引入 tslib 里的辅助工具函数
    // "downlevelIteration": true,            // 当 target 为 ES5 或 ES3 时，为 'for-of', spread, destructuring 中的迭代器提供完全支持
    // "isolatedModules": true,               // 将每个文件作为单独的模块（类似 'ts.transpileModule'）。默认 true，它不可以和 declaration 同时设定

    /* Strict Type-Checking Options */
    "strict": true,                        // 启用所有严格类型检查选项，为true则会同时开启下面这几个严格类型检查
    // "noImplicitAny": true,                 // 不允许隐式的 any 类型，默认 true
    // "strictNullChecks": true,              // 启用严格的空值检查。null 和 undefined 不能赋给非这两种类型的值，别的类型也不能赋给他们，除了 any。还有个例外就是 undefined 可以赋值给 void
    // "strictFunctionTypes": true,           // 启用对函数类型的严格检查
    // "strictBindCallApply": true,           // 对 bind、call 和 apply 绑定的方法的参数进行严格检测
    // "strictPropertyInitialization": true,  // 在类中启用属性初始化的严格检查。检查类的非 undefined 属性是否已在构造函数里初始化，需开启 strictNullChecks
    // "noImplicitThis": true,                // this 表达式隐含 any 类型时报错
    // "alwaysStrict": true,                  // 以严格模式解析并为每个源文件加入 'use strict'

    /* Additional Checks */
    // "noUnusedLocals": true,                // 定义了但未使用的局部变量
    // "noUnusedParameters": true,            // 函数中未使用的参数
    // "noImplicitReturns": true,             // 检查函数是否有返回值
    // "noFallthroughCasesInSwitch": true,    // 检查 switch 中是否有 case 没有使用 break 跳出 switch

    /* Module Resolution Options */
    // "moduleResolution": "node",            // 选择模块解析策略: 'node' (Node.js) 或 'classic' (TypeScript pre-1.6)
    // "baseUrl": "./",                       // 用于解析非绝对模块名称的基本目录
    // "paths": {},                           // 设置模块名称相对于 baseUrl 的路径映射
    // "rootDirs": [],                        // 运行时项目结构的根文件夹列表
    // "typeRoots": [],                       // 包含类型定义的文件夹列表。只有在这里列出的声明文件才会被加载
    // "types": [],                           // 包含在编译中的类型声明文件。只有在这里列出的模块声明文件才会被加载
    // "allowSyntheticDefaultImports": true,  // 允许从没有默认导出的模块进行默认导入。这不影响编译结果，只影响类型检查
    "esModuleInterop": true,               // 通过为导入内容创建命名空间，实现 CommonJS 和 ES 模块之间的互操作性，蕴含 'allowSyntheticDefaultImports'
    // "preserveSymlinks": true,              // 不解析符号链接的真实路径
    // "allowUmdGlobalAccess": true,          // 允许从模块访问 UMD 全局

    /* Source Map Options */
    // "sourceRoot": "",                      // 指定调试器应该找到 ts 文件而不是源文件位置。这个值会被写进 .map 文件里
    // "mapRoot": "",                         // 指定调试器找到映射文件而非生成文件的位置
    // "inlineSourceMap": true,               // 将 map 内容和 js 文件编译在同一个 js 文件中。map 的内容会以 '//# sourceMappingURL=' 拼接 base64 字符串的形式插入在 js 文件底部
    // "inlineSources": true,                 // 进一步将 .ts 文件的内容也包含到输出文件中。需设置 '--inlineSourceMap' 或 '--sourceMap'

    /* Experimental Options */
    // "experimentalDecorators": true,        // 启用实验性支持：ES7 装饰器的
    // "emitDecoratorMetadata": true,         // 启用实验性支持：为装饰器提供类型元数据。通过 Reflect 提供的静态方法获取元数据，需引入 ES2015.Reflect 这个库

    /* Advanced Options */
    "forceConsistentCasingInFileNames": true  // 不允许对同一文件进行大小写不一致的引用
  },
  // "files": [],                             // 只编译指定文件，不能使用 * ? **/ 等通配符
  // "include": [],                           // 指定编译的文件或文件夹，可用通配符，例: './src'
  // "exclude": [],                           // 排除文件或文件夹，可用通配符
  // "extends": "",                           // 指定一个其他的 tsconfig.json 文件路径，来继承和覆盖当前文件定义的配置
  // "compileOnSave": true,                   // 在文件保存时编译，需编辑器支持
  // "references": [],                        // 指定要引用的项目
}
```