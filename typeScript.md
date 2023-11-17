`数据类型`

**1.基本数据类型**

略

**2.对象的类型-接口**

在ts中，使用接口来定义对象的类型。在面向对象语言中，接口是对行为的抽象，具体如何行动需要由类去实现。接口的首字母一般大写。定义的变量比接口少了属性或者多了属性都是不被允许的。

接口的可选属性：

```typescript
interface Person {
    name: string;
    age?: number
}
let tom:Person = {
    name: 'xxx'
}
```

接口的任意属性：

```typescript
interface Person {
    name: string;
    age?: number;
    [propName:string]: any
}
let tom:Person = {
    name: 'tom',
    gender: 'male'
}
```

一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

接口的只读属性：

对象中的一些字段只能在创建的时候被赋值，可以使用readonly定义只读属性：

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};
```

**3.数组类型**

类型+方括号

```typescript
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

数组泛型

```typescript
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

接口表示数组（几乎不使用）

```typescript
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

**4.函数类型**

函数的输入和输出在ts中都要进行约束

```typescript
function sum(x: number, y: number): number {
    return x + y;
}
```

输入多余的或者少于要求的参数都是不被允许的

**可选参数**

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

可选参数必须接在必需参数后面。可选参数后面不允许在出现必需参数

```typescript
function buildName(firstName?: string, lastName: string) {
    if (firstName) {
        return firstName + ' ' + lastName;
    } else {
        return lastName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName(undefined, 'Tom');

// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```

**参数默认值**

ts会将添加了默认值得参数识别为可选参数：

```typescript
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

此时就不受「可选参数必须接在必需参数后面」的限制了：

```typescript
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

`类型推论`

如果没有明确得指定类型，那么ts会依照类型推论的规则推断出一个类型

已下代码虽然没有指定类型，但是会在编译的时候报错：

```typescript
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

事实上，它等价于：

```typescript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

如果在定义的时候没有赋值，不管之后有没有赋值，都会被推断成any类型而完全不被类型检查

`联合类型`

联合类型表示取值可以为多种类型中的一种

```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

当ts不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法

```typescript
function getLength(something: string | number): number {
    return something.length;
}
// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

`类型断言`

//todo

`元组`

数组合并了相同类型的对象，而元组合并了不同类型的对象

```typescript
let tom: [string, number] = ['Tom', 25];
```

`泛型`

泛型是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

example：

```typescript
function createArray(length: number, value: any): Array<any> {
    let result = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

这段代码编译不会报错，但是他并没有准确的定义返回值的类型。允许数组的每一项都为任意类型，但是我们的预期是数组中的每一项都应该是value的类型

```typescript
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

`Object和object`

大写的Object类型代表js语言里面的广义对象。所有可以转成对象的值，都是Object类型，这囊括了几乎所有的值

```javascript
let obj:Object;
 
obj = true;
obj = 'hi';
obj = 1;
obj = { foo: 123 };
obj = [1, 2];
obj = (a:number) => a + 1;
```

