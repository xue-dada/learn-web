## vue入门

https://vue3.chengpeiquan.com/guide.html#%E5%88%9D%E5%A7%8B%E5%8C%96%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE

1、初始化项目

```shell
mkdir node-demo
cd node-demo
npm init -y 
# 加上 -y 参数，这样会以 Node 推荐的答案帮你快速生成项目信息。
# 新建index.js console.log("hello world")
# scripts 中写 "dev":"node index"
npm run dev
npm view vue 查看版本
```

2、了解package.json

```
{
  "name": "node-demo",
  "version": "1.0.0",
  "description": "A demo about Node.js.",
  "main": "index.js",  // 项目的入口文件
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "chengpeiquan",
  "license": "MIT"
}
```

**scripts** 指定运行脚本的命令缩写，常见的如 `npm run build` 等命令就在这里配置，详见 [脚本命令的配置](https://vue3.chengpeiquan.com/guide.html#脚本命令的配置)

|                 |                                                              |
| --------------- | ------------------------------------------------------------ |
| dependencies    | 记录当前项目的生产依赖，安装 npm 包时会自动生成，详见：[了解包和插件](https://vue3.chengpeiquan.com/guide.html#了解包和插件) |
| devDependencies | 记录当前项目的开发依赖，安装 npm 包时会自动生成，详见：[了解包和插件](https://vue3.chengpeiquan.com/guide.html#了解包和插件) |
|                 |                                                              |

3、模块化设计

### 用 CommonJS 设计模块

略

###  用 ES Module 设计模块

https://vue3.chengpeiquan.com/guide.html#%E9%BB%98%E8%AE%A4%E5%AF%BC%E5%87%BA%E5%92%8C%E5%AF%BC%E5%85%A5-1

#### 基本语法

ESM 使用 `export default` （默认导出）和 `export` （命名导出）这两个语法导出模块，和 CJS 一样， ESM 也可以导出任意合法的 JavaScript 类型，例如：字符串、布尔值、对象、数组、函数等等。

使用 `import ... from ...` 导入模块，在导入的时候，如果文件扩展名是 `.js` 则可以省略文件名后缀，否则需要把扩展名也完整写出来。

#### 默认导出和导入

默认导出的意思是，一个模块只包含一个值；而导入默认值则意味着，导入时声明的变量名就是对应模块的值。

```javascript
// src/cjs/module.cjs
// 默认导出
module.exports = 'Hello World'

// src/cjs/index.cjs
// 默认导入
const m = require('./module.cjs')
console.log(m)

// 默认导出函数
// src/cjs/module.cjs
module.exports = function foo() {
  console.log('Hello World')
}
// 默认导入函数
// src/cjs/index.cjs
const m = require('./module.cjs')
m()
```

#### 命名导出和导入

命名导出，这样既可以导出多个数据，又可以统一在一个文件里维护管理，命名导出是先声明多个变量，然后通过 `{}` 对象的形式导出。

```javascript
// 导出
// src/cjs/module.cjs
function foo() {
  console.log('Hello World from foo.')
}

const bar = 'Hello World from bar.'

module.exports = {
  foo,
  bar,
}

// 导入
// src/cjs/index.cjs
const { foo, bar } = require('./module.cjs')
foo()
console.log(bar)
```

#### 导入时重命名

```javascript
// src/cjs/module.cjs
function foo() {
  console.log('Hello World from foo.')
}

const bar = 'Hello World from bar.'

module.exports = {
  foo,
  bar,
}


// 导出重命名
// src/cjs/index.cjs
const {
  foo: foo2,  // 这里进行了重命名
  bar,
} = require('./module.cjs')

// 就不会造成变量冲突
const foo = 1
console.log(foo)

// 用新的命名来调用模块里的方法
foo2()

// 这个不冲突就可以不必处理
console.log(bar)
```

### TIP

*注意这里我使用了 `.mjs` 文件扩展名，因为默认情况下， Node 需要使用该扩展名才会支持 ES Module 规范。*

*你也可以在 package.json 里增加一个 `"type": "module"` 的字段来使 `.js` 文件支持 ESM ，但对应的，原来使用 CommonJS 规范的文件需要从 `.js` 扩展名改为 `.cjs` 才可以继续使用 CJS 。*

*为了减少理解上的门槛，这里选择了使用 `.mjs` 新扩展名便于入门，可以在 [了解 package.json](https://vue3.chengpeiquan.com/guide.html#了解-package-json) 部分的内容了解更多。*

