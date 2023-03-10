## 一. 前端模块化

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

