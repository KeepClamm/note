

## 前端模块化

通常一个文件就是一个模块，有自己的作用域，只向外暴露特定的变量和函数。目前流行的JS模块化规范有CommonJS、AMD、CMD以及ES6的模块系统。

### CommonJS

Node.js是commonJS规范的主要实践者，它有四个重要的环境变量为模块化的实现提供支持：module、exports、require、global。使用的时候，用module.exports定义当前模块对外输出的接口，用require加载模块

```javascript
// 定义模块
let num = 0
function add(a, b) {
    return a + b
}
module.exports = { // 向外暴露的函数、变量
    num,
    add
}
// 引用
let math = require('./math')
math.add(1, 2)
```

CommonjJS用 **同步**的方式加载模块。在服务端，模块文件都存在本地磁盘，读取非常快，所以不会有问题。但是当宿主对象为浏览器的时候，限于网络原因，更合理的方案是使用异步加载。

### AMD和require.js

AMD规范采用异步方式加载模块，模块的加载不会影响它后面语句的执行。所有依赖这个模块的语句，都定义在一个回调函数中，等加载完成之后，这个回调函数才会执行。用require.config()指定引用路径。用define()定义模块，用require()加载模块

### CMD和sea.js

CMD是另外一种JS模块化规范，它与AMD类似。不同点在于：AMD推从依赖前置、提前执行。CMD推崇依赖就近、延迟执行。

### ES6 Module

export 命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能

```javascript
// 导出
let num = 0 
let add = function(a, b) {
	return a + b
}
export {num, add}
// 引用
import {num, add} from './math'
function test(ele) {
    ele.text = add(99 + num)
}

```

使用import命令的时候，用户需要知道所要加载的变量名或函数名。ES6还提供了export default命令，为模块指定默认输出，对应的import语句不需要使用大括号。

```javascript
export default {num, add} 

import math from './math'
```

ES6的模块不是对象，import命令会被js引擎静态分析，在编译时就引入模块代码，而不是在代码运行时加载。所有无法实现条件加载。也正是由于这个原因，使得静态分析成为可能。

### ES6和CommonJS的差异

1. CommonJS模块输出的是值得拷贝。模块内部得变化影响不到这个值

​		ES6模块是动态引用，不会缓存值，模块里面的变量绑定其所在的模块

2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

   运行时加载: CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。

   编译时加载: ES6 模块不是对象，而是通过 `export` 命令显式指定输出的代码，`import`时采用静态命令的形式。即在`import`时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

CommonJS 加载的是一个对象（即`module.exports`属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。



## 变量的解构赋值

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构。

### 数组的解构赋值

```javascript
let [ , , third] = ["foo", "bar", "baz"];
```

### 对象的解构赋值

```javascript
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
```

变量必须与属性同名，才能取到正确的值。如果解构失败，变量的值等于undefined

### 函数参数的解构赋值

```javascript
function add([x, y]){
  return x + y;
}
add([1, 2]); // 3
```



函数参数的解构也能使用默认值

```javascript
function move({x = 0, y = 0} = {}) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

## CSS选择器

### 关系选择符

| 选择符 | 选择符名称 | 描述                                                 |
| ------ | ---------- | ---------------------------------------------------- |
| `E F`  | 包含选择符 | 选择所有被 E 元素包含的 F 元素（能命中所有后代元素） |
| `E>F`  | 子选择符   | 选择所有作为 E 元素的子元素 F（只能命中子元素）      |
| `E+F`  | 相邻选择符 | 选择紧贴在 E 元素之后 F 元素                         |
| `E~F`  | 兄弟选择符 | 选择 E 元素所有兄弟元素 F                            |

### 属性选择符

| 选择符          | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| `E[att]`        | 选择具有 `att` 属性的 E 元素。                               |
| `E[att='val']`  | 选择具有 `att` 属性且属性值等于 `val` 的 E 元素。            |
| `E[att~='val']` | 选择具有 `att` 属性且属性值为一用空格分隔的字词列表，其中一个等于 `val` 的 E 元素。 |
| `E[att^='val']` | 选择具有 `att` 属性且属性值为以 `val` 开头的字符串的 E 元素。 |
| `E[att$='val']` | 选择具有 `att` 属性且属性值为以 `val` 结尾的字符串的 E 元素。 |
| `E[att*='val']` | 选择具有 `att` 属性且属性值为包含 `val` 的字符串的 E 元素。  |
| `E[att|='val']` | 选择具有 `att` 属性且属性值为以 `val` 开头并用连接符 `-` 分隔的字符串的 E 元素，如果属性值仅为 `val`，也将被选择。 |

```css
.div {
  /* 表示存在 class 属性并且以 title 开头的元素 */
  [class^='title'] {
    margin-bottom: 8px;
  }
}
```

## 可选链运算符(?.)

可选链运算符允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。

`语法`

```javascript
obj.val?.prop
obj.val?.[expr]
obj.func?.(args)
```

`描述`

通过连接的对象的引用或函数可能是undefined或null时，可选链运算符提供了一种方法来简化被连接对象的值的访问。

比如，一个存在嵌套结构的对象，不使用可选链的话，查找一个深度嵌套的子属性时，需要验证之间的引用。

```javascript
let newObj = obj.first && obj.first.second
```

为了避免报错，在访问obj.first.second之前，要保证obj.first的值既不是null，也不是undefined。如果只是直接访问obj.first.second，而不对obj.first进行校验，则有可能抛出错误。

```javascript
let newObj = obj.first?.second
```

## this.$nextTick

> 在下次DOM更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，回调函数获取更新后的dom再渲染出来；nextTick类似于一个非常高级的定时器，自动追踪DOM刚更新，更新好了就触发。

什么时候需要nextTick？

data改变，更新DOM是异步的；DOM更新是异步的，Vue响应式的特征，修改数据后，页面自动更新，而更新DOM这个操作是异步的；这个时候使用nextTick()，回调函数会在下一次DOM更新完毕后执行。

`真实案例：当新增和修改共用同一个el-dialog，按以下步骤会有bug：点击修改-->el-form回显数据-->关闭el-dialog-->点击新增-->el-dialog还有原来的数据，这样是不合理的。原因在于关闭dialog时form的 resetFields 没生效；原因是还未获取到DOM；解决方法是在修改赋值时，使用$nextTick()回调推迟到下一个 DOM 更新周期之后执行`

## vite.config.ts

defineConfig函数是在vite中用于创建配置对象的常见方法，它通常用于定义开发环境和生产环境的配置选项。

`base:`用于指定项目的基础路径，通常用于将项目部署到子路径的情况。

`build:`包含了构建相关的配置选项，比如输出路径、是否开启压缩、是否开启代码分割。

`server:`用于配置开发服务器，包括主机地址、端口号、代理设置等。

`resolve:`用于配置模块解析规则，包括路径别名、模块文件后缀名的解析顺序等。

`plugins:`用于配置vite插件，可以对代码进行转换、引入第三方等。

`css:`用于配置CSS相关的选项，比如预处理器、样式模块化等。

`sbuild:`用于配置esbuild相关的选项，比如自定义JSX配置、代码压缩优化等。

```typescript
//导出一个默认的配置对象，其中包括了 Vite 的各种配置选项。
export default defineConfig({
    root, //项目根目录（index.html 文件所在的位置） 默认： process.cwd()
    base: '/', //  开发或生产环境服务的公共基础路径：默认'/'   1、绝对 URL 路径名： /foo/；  2、完整的 URL： https://foo.com/； 3、空字符串或 ./（用于开发环境）
    publicDir: resolve(__dirname, './dist'), //默认'public'  作为静态资源服务的文件夹  (打包public文件夹会没有，里面得东西会直接编译在dist文件下)
    assetsInclude: resolve(__dirname, './src/assets'), // 静态资源处理
    
    /*****配置项目的构建过程******/
    build: {
        outDir: 'dist', // 构建输出目录
        minify: true, // 是否压缩代码
        sourcemap: true, // 是否生成 source map
    },
    
    /*****定义全局变量******/
    define: {
        MENU_PATH: `"path"`,
        MENU_SHOW: `"isShowOnMenu"`,
        MENU_KEEPALIVE: `"keepAlive"`,
        MENU_KEY: `"key"`,
        MENU_ICON: `"icon"`,
        MENU_TITLE: `"title"`,
        MENU_CHILDREN: `"children"`,
        MENU_PARENTKEY: `"parentKey"`,
        MENU_ALLPATH: `"allPath"`,
        MENU_PARENTPATH: `"parentPath"`,
        MENU_LAYOUT: `'layout'`,
        __IS_THEME__: `${process.env.REACT_APP_COLOR === "1"}`,
        CUSTOMVARLESSDATA: `${JSON.stringify(customVarLessJson)}`
    },
    
    /******配置插件******/
    plugins: [
        ReactRouterGenerator({
          outputFile: resolve(".", "./src/router/auto.jsx"),
          isLazy: true,
          comKey: "components"
        }),
       react(),
    ],
    
 
    /******配置模块解析的规则******/
    resolve: {
      //路径使用别名
      alias: {
        '@': fileURLToPath(new URL('./src', import.meta.url)),
      },
      //引入文件的后缀名称，可以省略。如果出现同名，按照数组加载的优先顺序
      extensions: ['.mjs', '.js', '.ts', '.jsx', '.tsx', '.json', '.vue'],
    },
    
    /******配置开发服务器******/
    server: {
      port: 3333,// 端口号
      open: true,// 启动时自动在浏览器打开
      https: true,// 是否开启 https
      host: true, // 监听所有地址
      cors: false, //为开发服务器配置 CORS
      fs: {
        // 可以为项目根目录的上一级提供服务
        allow: [".."],
      },
      //配置自定义代理规则
      proxy: {
          '^/api': {
                target: "https://z3web.cn",
                changeOrigin: true,
                rewrite: (path) => {
                return path.replace("/api", "/api/react-ant-admin")
            }
       },
    },
    
    /*****配置CSS相关的选项********/
    css: {
      //配置了对 SCSS 的处理选项
      preprocessorOptions: {
        scss: {
          //引入了全局的 SCSS 文件 global.scss
          additionalData: `@import "./src/assets/css/global.scss";`,
        },
      },
      // 可以查看 CSS 的源码
      devSourcemap: true
    },
    
    /****配置 esbuild 相关的选项******/
    esbuild: {
      // 自定义 JSX 配置
      jsxFactory: 'h', //自定义的 JSX 工厂函数为 h，这在一些非 React 框架中可能会用到。
      jsxFragment: 'Fragment', //指定了 JSX 的 Fragment 为 Fragment
      jsxInject: `import React from 'react'` //是否开启 JSX 转换
    }
})
```

## vue-router

`命名路由`

在配置路由的时候，给路由添加名字，访问时就可以动态的根据名字来进行访问

```javascript
const router = new Router({
    routes: [{
        path: '/home',
        name: 'home',
        component: Home
    }]
})
```

```vue
<router-link :to="{name: 'home'}">Home</router-link>
```

`动态路由匹配`

把某种模式匹配到的所有路由，全部映射到同个组件。例如，有一个user组件，对于ID不同的用户，都要使用这个组件来渲染。可以使用动态路由来达到这个效果。

```javascript
const router = new Router({
    routes: [{
        path: '/user/:id',
        name: 'user',
        component: User
    }]
})
```

```vue
<router-link :to="{name: 'user', params{id:1}}"></router-link>
```

```vue
{{ $route.params.id }}
```

## HTTP协议

`HTTP请求方法`

包括：GET、HEAD、POST、PUT、PATCH、DELETE、OPTIONS

- GET:获取服务器的指定资源
- HEAD：与GET方法一样，都是发出一个获取服务器指定资源的请求，但服务器只会返回Header而不会返回Body。用于确认URI的有效性及资源更新的日期时间等。一个典型应用是下载文件时，先通过HEAD方法获取Header，从中读取文件大小Content-Length；然后再配合Range字段，分片下载服务器资源
- POST：提交资源到服务器
- PUT：替换整个目标资源
- PATCH：替换目标资源的部分内容
- DELETE：删除指定的资源

| GET             | POST                                     |                                   |
| --------------- | ---------------------------------------- | --------------------------------- |
| 应用            | 获取服务器的指定数据                     | 添加 / 修改服务器的数据           |
| 历史记录 / 书签 | 可保留在浏览器历史记录中，或者收藏为书签 | 不可以                            |
| Cacheable       | 会被浏览器缓存                           | 不会缓存                          |
| 幂等            | 幂等，不会改变服务器上的资源             | 非幂等，会对服务器资源进行改变    |
| 后退 / 刷新     | 后退或刷新时，GET 是无害的               | 后退或刷新时，POST 会重新提交表单 |
| 参数位置        | query 中（直接明文暴露在链接中）         | query 或 body 中                  |
| 参数长度        | 2KB（2048 个字符）                       | 无限制                            |

`HTTP的长连接与短连接`

**HTTP/1.0默认使用的是短连接。**也就是说，浏览器每请求一个静态资源，就建立一次连接，任务结束就中断连接。

**HTTP/1.1默认使用的是长连接。**长连接是指再一个网页打开期间，所有网络请求都使用同一条已经建立的连接。当没有数据发送时，双方需要发检测包已维持此连接。长连接不会永久保持连接，而是由一个保持时间。实现长连接要客户端和服务端都支持长连接。

长连接的有点：TCP三次握手时会有1.5RTT的延迟，以及建立连接后慢启动特性，当请求频繁时，建立和关闭TCP连接会浪费时间和带宽，而重用一条已有的连接性能更好。

长连接的缺点：长连接会占用服务器的资源

`HTTP/1.0、HTTP/1.1、HTTP/2.0的变化`

**HTTP/1.0-构建可扩展性**

- 在请求中新增了协议版本信息
- 引入了HTTP头的概念
- 在响应中新增了状态码
- 默认使用短连接

**HTTP/1.1-标准化的协议**

- 默认支持长连接
- 引入额外的缓存机制，如etag if-none-match 等缓存头
- 新增了24个错误响应状态响应码
- 支持响应分块（断点续传）
- Host头，允许不同域名配置在同一个IP地址上

**HTTP/2.0-为了更优异的表现**

HTTP/2.0的三大特性：Header压缩、服务端推送、多路复用

**Header压缩**

HTTP/1.1每次通信都会携带Header信息用于描述资源属性。但headers在一系列请求中常常是相似的。HTTP/2.0中，对于Header中相同的数据不会在每次通信中重新发送，而是采用追加或替换的方式。

**服务端推送**

服务器可以对一个客户端请求发送**多个**响应。服务器向客户端推送资源无需客户端明确的请求。

服务端根据客户端的请求，提前推送额外的资源给客户端。比如在发送页面 HTML 时主动推送其它 CSS/JS 资源，而不用等到浏览器解析到相应位置，发起请求再响应。

服务端推送可以减轻数据传输的冗余步骤，同时加快页面响应速度，提升用户体验。

**多路复用**

HTTP/2.0 引入了**多路复用**，通过同一个连接发起多个请求，服务端可以**并行**地传输数据。基于二进制分帧层，HTTP/2.0 可以同时交错发送多个消息中的帧，接收端可以根据帧中的流标识符和顺序标识，重新组装数据。

多路复用使用同一个 TCP 连接并发处理同一域名下的所有请求，可以减少 TCP 建立连接带来的时延。此外多路复用代替了 HTTP/1.x 中的顺序和阻塞机制，实现了真正的并行传输，可以避免 HTTP/1.x 中的队头阻塞问题，极大的提高传输效率。
