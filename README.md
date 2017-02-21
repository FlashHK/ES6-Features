# ES6常用特性

## 一、let + const - 块级作用域

### 不存在变量提升

```javascript
// var 的情况
console.log(foo) // 输出undefined
var foo = 2

// let 的情况
console.log(bar) // 报错ReferenceError
let bar = 2
```

### 块级作用域
> ES5只有全局作用域和函数作用域，没有块级作用域

> ES6{}花括号内为块级作用域

```javascript
// ES5
var tmp = new Date()

function f() {
  console.log(tmp)
  if (false) {
    var tmp = "hello world"
  }
}

f() // undefined
```

```javascript
// ES6
let tmp = new Date()

function f() {
  console.log(tmp)
  if (false) {
    let tmp = "hello world"
  }
}

f() // Thu Feb 09 2017 16:14:40 GMT+0800 (中国标准时间)
```

### const声明一个只读的常量。一旦声明，常量的值就不能改变。

## 二、template strings - 模板字符串

> 反引号（`）标识

```javascript
// 基本的字面量字符串创建
// bad
const str = 'a string containing ' + 'single' + ' quotes'
const str = `a string containing single quotes`

// good
const str = "a string containing 'single' quotes"
const str = 'a string containing "single" quotes'
const str = `a string containing ${obj.name} quotes`
```

```javascript
// 模板字符串之中还能调用函数
function fn() {
  return "Hello World"
}
const str = `a string containing ${fn()} quotes`
```

```javascript
// 如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。
const str = `a \`strin\` containing ${obj.name} quotes`
```

## 三、enhanced object literals - 增强对象字面量
> 对象字面量扩展了以下特性，支持在构造时设置原型，foo: foo赋值的简写，方法定义，调用父方法，使用表达式计算属性名。

```javascript
const obj = {
    // __proto__
    __proto__: theProtoObj,
    // handler: handler 简写
    handler,
    toString() {
     // 旧 return 'd' + obj.__proto__.toString()
     return "d " + super.toString()
    },
    // 表达式计算属性名
    [`a key named ${k}`]: 42
}
```

## 四、destructuring - 解构

> ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构

```javascript
const [a, b, c] = [1, 2, 3]
const { bar, foo } = { foo: "aaa", bar: "bbb" }
const [a, b, c, d, e] = 'hello'
```

### 函数的参数也可以使用解构赋值

```javascript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```


## 五、default + rest + spread - 默认赋参+不定参数+参数展开

```javascript
function f(x = 1, y) {
  return [x, y]
}

f() // [1, undefined]
f(2) // [2, undefined])
// f(, 1) // 报错
f(undefined, 1) // [1, 1]
```

### 参数默认值可以与解构赋值的默认值，结合起来使用

```javascript
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined, 5
foo({x: 1}) // 1, 5
foo({x: 1, y: 2}) // 1, 2
foo() // TypeError: Cannot read property 'x' of undefined
```

```javascript
function foo({x, y = 5} = {}) {
  console.log(x, y);
}
```

### 通常情况下，定义了默认值的参数，应该是函数的尾参数。
```javascript
function f(x = 1, y) {
  return [x, y];
}

f() // [1, undefined]
f(2) // [2, undefined])
f(, 1) // 报错
f(undefined, 1) // [1, 1]
```

### rest参数
```javascript
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10
```

### 注意，rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。
```javascript
// 报错
function f(a, ...b, c) {
  // ...
}
```

### 扩展运算符（spread）是三个点（...）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```javascript
// 替代数组的apply方法

// ES5的写法
Math.max.apply(null, [14, 3, 77])

// ES6的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

### 扩展运算符可以与解构赋值结合起来，用于生成数组。

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
```

### 如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

```javascript
const [...butLast, last] = [1, 2, 3, 4, 5];
// 报错

const [first, ...middle, last] = [1, 2, 3, 4, 5];
// 报错
```

## 六、math + number + string + array + object APIs

```javascript
// 用来判断给定的参数是否为整数
Number.isInteger(Infinity) // false
```
```javascript
// 检测传入的值是否是 NaN
// bad
isNaN("NaN") //true
isNaN(undefined) //true
//good
Number.isNaN("NaN") // false
Number.isNaN(undefined);  // false

// 可以把isNaN看做
isNaN = function(value) {
    Number.isNaN(Number(value));
}

// es5
表达式(x != x) 来检测变量x是否是NaN会更加可靠
```

```javascript
// 将数字的小数部分去掉，只留整数部分
Math.trunc(0.923)
```
```javascript
// 判断一个字符串是否包含在另一个字符串中
"abcde".includes("cd") // true
```
```javascript
// 从类似数组或可迭代对象创建一个新的数组实例
Array.from(document.querySelectorAll('*')) // 返回真实的数组，可以使用数组的相关方法
// 生成一个数字序列
Array.from({ length: 5 }, (v, k) => k) // [0, 1, 2, 3, 4]
```

```javascript
// 将一个数组的所有元素从开始索引填充到具有静态值的结束索引
[0, 1, 0, 1, 1].fill(0)            // [0, 0, 0, 0, 0]
```

```javascript
// 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
```

```javascript
// 拷贝对象
Object.assign({a:1}, { a: 2,b:3 })

const obj = { 0 : "a", 1 : "b", 2 : "c"}
console.log(Object.keys(obj)) // ["0", "1", "2"]
console.log(Object.values(obj)) // ["a", "b", "c"]
```

```javascript
const myArray = [1, 2, 3]
myArray.xx = 10
// bad
// 会遍历自定义属性,浏览器表现不一致，按照随机顺序遍历数组元素
for (const key in myArray) {
  console.log(key, myArray[key])
}
// good
// 与forEach()不同的是，它可以正确响应break、continue和return语句
for (const key of myArray) {
  console.log(key)
}
```

## 七、promises对象

```javascript
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

```javascript
new Promise(resolve => resolve('foo'))
  .then((res) => {
    console.log(res)
  })
  .catch((err) => {
    console.log(err)
  })


Promise.resolve('foo')
  .then((res) => {
    console.log(res)
  })
  .catch((err) => {
    console.log(err)
  })

Promise.reject('foo')
  .catch((err) => {
    console.log(err)
  })
```

## 八、classes - 类

```javascript
// Class不存在变量提升（hoist），这一点与ES5完全不同。
new Foo(); // ReferenceError
class Foo {}
```

```javascript
class Animal {
  // 静态属性，不需要实例化直接调用 Animal.head
  static head = 1

  constructor() {
    this.name = ''
    this.ear = 2
  }

  say() {
    console.log(`say ${this.name}`)
  }

  // 静态方法，不需要实例化直接调用 Animal.do()
  static do() {
    console.log('do...')
  }
}

class Cat extends Animal {
  constructor() {
    super()

    this.name = 'cat'
    this.foot = 4
  }

  say() {
    // super.say()
    console.log(`Cat say ${this.name}`)
  }

  get animalName() {
    return this.name
  }

  set animalName(_name) {
    this.name = `animal_${_name}`
  }
}

const cat = new Cat()
console.log(cat.head, cat.ear, cat.foot)
console.log(Cat.head)
// get set
console.log(cat.animalName)
cat.animalName = 'cat'
console.log(cat.animalName)
Animal.do()
Cat.do()
cat.say()
```

## 九、modules - 模块

```javascript
// bad
export function foo() {}

// good
export default function foo() {}
```

```javascript
// bad
import foo from 'foo';
// … some other imports … //
import { named1, named2 } from 'foo'

// good
import foo, { named1, named2 } from 'foo'

// good
import foo, {
  named1,
  named2,
} from 'foo'
```

# 学习链接

[ECMAScript 6 入门](http://es6.ruanyifeng.com/#README)

[ECMAScript 6 特性](https://github.com/fengzilong/es6features-zhCN)

[Airbnb JavaScript 编码规范](https://github.com/yuche/javascript)
