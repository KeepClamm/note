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

