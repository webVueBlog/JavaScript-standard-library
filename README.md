# 《前端进阶 JavaScript 标准库》 

前端进阶必看的JavaScript 标准库

https://webvueblog.github.io/JavaScript-standard-library/

<img width="300px" src="https://user-images.githubusercontent.com/59645426/187019807-7785b3c9-9931-4bca-a28f-5e508a4c9839.jpg"/>

## JavaScript 标准内置对象

这里的术语"全局对象"（或标准内置对象）不应与global 对象混淆。这里的"全局对象"指的是处在全局作用域里的多个对象。

global 对象可以在全局作用域里通过使用this访问到（但只有在 ECMAScript 5 的非严格模式下才可以，在严格模式下得到的是 undefined）。其实全局作用域包含全局对象中的属性，包括它可能继承来的属性。

全局作用域中的其他对象则可由用户的脚本创建，或由宿主程序提供。浏览器环境中所提供的宿主对象的说明可以在这里找到：API 参考。

## 标准内置对象分类

### 值属性

这些全局属性返回一个简单值，这些值没有自己的属性和方法。

- Infinity
- NaN
- undefined
- globalThis

### 函数属性

全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。

- eval()
- uneval()
- isFinite()
- isNaN()
- parseFloat()
- parseInt()
- decodeURI()
- decodeURIComponent()
- encodeURI()
- encodeURIComponent()

### 基本对象

顾名思义，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。

- Object
- Function
- Boolean
- Symbol

### 错误对象

错误对象是一种特殊的基本对象。它们拥有基本的 Error 类型，同时也有多种具体的错误类型。

- Error
- AggregateError
- EvalError
- InternalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError

### 数字和日期对象

用来表示数字、日期和执行数学计算的对象。

- Number
- BigInt
- Math
- Date

### 字符串

用来表示和操作字符串的对象。

- String
- RegExp

### 可索引的集合对象

这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。

- Array
- Int8Array
- Uint8Array
- Uint8ClampedArray
- Int16Array
- Uint16Array
- Int32Array
- Uint32Array
- Float32Array
- Float64Array
- BigInt64Array
- BigUint64Array

### 使用键的集合对象

这些集合对象在存储数据时会使用到键，包括可迭代的Map 和 Set，支持按照插入顺序来迭代元素。

- Map
- Set
- WeakMap
- WeakSet

### 结构化数据

这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON（JavaScript Object Notation）编码的数据。

- ArrayBuffer
- SharedArrayBuffer
- Atomics
- DataView
- JSON

### 控制抽象对象

控件抽象可以帮助构造代码，尤其是异步代码（例如，不使用深度嵌套的回调）。

- Promise
- Generator
- GeneratorFunction
- AsyncFunction

### 反射

- Reflect
- Proxy

### 国际化

ECMAScript 核心的附加功能，用于支持多语言处理。

- Intl
- Intl.Collator
- Intl.DateTimeFormat
- Intl.ListFormat
- Intl.NumberFormat
- Intl.PluralRules
- Intl.RelativeTimeFormat
- Intl.Locale

### WebAssembly- 

- WebAssembly
- WebAssembly.Module
- WebAssembly.Instance
- WebAssembly.Memory
- WebAssembly.Table
- WebAssembly.CompileError
- WebAssembly.LinkError (en-US)
- WebAssembly.RuntimeError

### 其他

- arguments

<hr>

## Object

Object 是 JavaScript 的一种 数据类型 。它用于存储各种键值集合和更复杂的实体。Objects 可以通过 Object() 构造函数或者使用 对象字面量 的方式创建

### 描述

在 JavaScript 中，几乎所有的对象都是 Object 类型的实例，它们都会从 Object.prototype 继承属性和方法，虽然大部分属性都会被覆盖（shadowed）或者说被重写了（overridden）。 除此之外，Object 还可以被故意的创建，但是这个对象并不是一个“真正的对象”（例如：通过 Object.create(null)），或者通过一些手段改变对象，使其不再是一个“真正的对象”（比如说：Object.setPrototypeOf）。

通过原型链，所有的 object 都能观察到 Object 原型对象（Object prototype object）的改变，除非这些受到改变影响的属性和方法沿着原型链被进一步的重写。尽管有潜在的危险，但这为覆盖或扩展对象的行为提供了一个非常强大的机制。

Object 构造函数为给定的参数创建一个包装类对象（object wrapper），具体有以下情况：

- 如果给定值是 null 或 undefined，将会创建并返回一个空对象
- 如果传进去的是一个基本类型的值，则会构造其包装类型的对象
- 如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址

当以非构造函数形式被调用时，Object 的行为等同于 new Object()。

### 从一个对象上删除一个属性

Object 自身没有提供方法删除其自身属性（Map 中的 Map.prototype.delete() 可以删除自身属性）为了删除对象上的属性，必须使用 delete 操作符

### 构造函数

Object()

创建一个新的 Object 对象。该对象将会包裹（wrapper）传入的参数

### 静态方法

Object.assign()
通过复制一个或多个对象来创建一个新的对象。

Object.create()
使用指定的原型对象和属性创建一个新对象。

Object.defineProperty()
给对象添加一个属性并指定该属性的配置。

Object.defineProperties()
给对象添加多个属性并分别指定它们的配置。

Object.entries()
返回给定对象自身可枚举属性的 [key, value] 数组。

Object.freeze()
冻结对象：其他代码不能删除或更改任何属性。

Object.getOwnPropertyDescriptor()
返回对象指定的属性配置。

Object.getOwnPropertyNames()
返回一个数组，它包含了指定对象所有的可枚举或不可枚举的属性名。

Object.getOwnPropertySymbols()
返回一个数组，它包含了指定对象自身所有的符号属性。

Object.getPrototypeOf()
返回指定对象的原型对象。

Object.is()
比较两个值是否相同。所有 NaN 值都相等（这与==和===不同）。

Object.isExtensible()
判断对象是否可扩展。

Object.isFrozen()
判断对象是否已经冻结。

Object.isSealed()
判断对象是否已经密封。

Object.keys()
返回一个包含所有给定对象自身可枚举属性名称的数组。

Object.preventExtensions()
防止对象的任何扩展。

Object.seal()
防止其他代码删除对象的属性。

Object.setPrototypeOf()
设置对象的原型（即内部 [[Prototype]] 属性）。

Object.values()
返回给定对象自身可枚举值的数组。

### 实例属性

Object.prototype.constructor
一个引用值，指向 Object 构造函数

Object.prototype.__proto__
指向一个对象，当一个 object 实例化时，使用该对象作为实例化对象的原型

### 实例方法

Object.prototype.__defineGetter__()
将一个属性与一个函数相关联，当该属性被访问时，执行该函数，并且返回函数的返回值。

Object.prototype.__defineSetter__()
将一个属性与一个函数相关联，当该属性被设置时，执行该函数，执行该函数去修改某个属性。

Object.prototype.__lookupGetter__()
返回一个函数，该函数通过给定属性的 Object.prototype.__defineGetter__() 得出。

Object.prototype.__lookupSetter__()
返回一个函数，该函数通过给定属性的 Object.prototype.__defineSetter__() 得出。

Object.prototype.hasOwnProperty()
返回一个布尔值，用于表示一个对象自身是否包含指定的属性，该方法并不会查找原型链上继承来的属性。

Object.prototype.isPrototypeOf()
返回一个布尔值，用于表示该方法所调用的对象是否在指定对象的原型链中。

Object.prototype.propertyIsEnumerable()
返回一个布尔值，用于表示内部属性 ECMAScript [[Enumerable]] attribute 是否被设置。

Object.prototype.toLocaleString()
调用 toString()。

Object.prototype.toString()
返回一个代表该对象的字符串。

Object.prototype.valueOf()
返回指定对象的原始值。

<hr>

## Array

JavaScript 的 Array 对象是用于构造数组的全局对象，数组是类似于列表的高阶对象。

1. 构造器： `Array()` 创建一个新的Array对象
2. 静态属性：`get Array[@@species]` 返回Array的构造函数
3. 静态方法：
   - `Array.from()` 从类数组对象或者可迭代对象中创建一个新的数组实例
   - `Array.isArray()` 用来判断某个变量是否是一个数组对象
   - `Array.of()` 根据一组参数来创建新的数组实例，支持任意的参数数量和类型 - `Array(1, 2, 3);    // [1, 2, 3]`
4. 实例属性：
   - `Array.prototype.length`: 数组中的元素个数
   - `Array.prototype[@@unscopables]`: 包含了所有 ES2015 (ES6) 中新定义的、且并未被更早的 ECMAScript 标准收纳的属性名。这些属性被排除在由 with 语句绑定的环境中
5. 实例方法：
   - `Array.prototype.at()`: at() 方法接收一个整数值并返回该索引的项目，允许正数和负数。负整数从数组中的最后一个项目开始倒数。(返回给定索引处的数组项。接受负整数，从最后一项开始计数。)
   - `Array.prototype.concat()`: 用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组
   - `Array.prototype.copyWithin()`: 浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度
   - `Array.prototype.entries()`: 返回一个新的 Array Iterator 对象，该对象包含数组中每个索引的键/值对
   - `Array.prototype.every()`: 测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值
   - `Array.prototype.fill()`: 用一个固定值填充一个数组中从起始索引到终止索引内的全部元素
   - `Array.prototype.filter()`: 创建一个新数组，其包含通过所提供函数实现的测试的所有元素
   - `Array.prototype.find()`: 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined
   - `Array.prototype.findIndex()`: 返回数组中满足提供的测试函数的第一个元素的索引。若没有找到对应元素则返回 -1
   - `Array.prototype.flat()`: 按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回
   - `Array.prototype.flatMap()`: 使用映射函数映射每个元素，然后将结果压缩成一个新数组
   - `Array.prototype.forEach()`: 对数组的每个元素执行一次给定的函数
   - `Array.prototype.includes()`: 判断一个数组是否包含一个指定的值，如果包含则返回 true，否则返回 false
   - `Array.prototype.indexOf()`: 返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回 -1
   - `Array.prototype.join()`: 将一个数组的所有元素连接成一个字符串并返回这个字符串
   - `Array.prototype.keys()`: 返回一个包含数组中每个索引键的 Array Iterator 对象
   - `Array.prototype.lastIndexOf()`: 返回指定元素在数组中的最后一个的索引，如果不存在则返回 -1
   - `Array.prototype.map()`: 返回一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值
   - `Array.prototype.pop()`: 从数组中删除最后一个元素，并返回该元素的值
   - `Array.prototype.push()`: 将一个或多个元素添加到数组的末尾，并返回该数组的新长度
   - `Array.prototype.reduce()`: 对数组中的每个元素执行一个由您提供的 reducer 函数（升序执行），将其结果汇总为单个返回值
   - `Array.prototype.reduceRight()`: 接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值
   - `Array.prototype.reverse()`: 将数组中元素的位置颠倒，并返回该数组。该方法会改变原数组
   - `Array.prototype.shift()`: 从数组中删除第一个元素，并返回该元素的值
   - `Array.prototype.slice()`: 提取源数组的一部分并返回一个新数组
   - `Array.prototype.some()`: 测试数组中是不是至少有一个元素通过了被提供的函数测试
   - `Array.prototype.sort()`: 对数组元素进行原地排序并返回此数组
   - `Array.prototype.splice()`: 通过删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容
   - `Array.prototype.toLocaleString()`: 返回一个字符串表示数组中的元素。数组中的元素将使用各自的 Object.prototype.toLocaleString() 方法转成字符串
   - `Array.prototype.toString()`: 返回一个字符串表示指定的数组及其元素。数组中的元素将使用各自的 Object.prototype.toString() 方法转成字符串
   - `Array.prototype.unshift()`: 将一个或多个元素添加到数组的头部，并返回该数组的新长度
   - `Array.prototype.values()`: 返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值
   - `Array.prototype[@@iterator]()`: 返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值

### 属性

> `Array.prototype[@@unscopables]`

Symbol 属性 @@unscopable 包含了所有 ES2015 (ES6) 中新定义的、且并未被更早的 ECMAScript 标准收纳的属性名。这些属性被排除在由 with 语句绑定的环境中。

语法：arr[Symbol.unscopables]

描述：

with 绑定中未包含的数组默认属性有：

- copyWithin()
- entries()
- fill()
- find()
- findIndex()
- includes()
- keys()
- values()

**`Array.prototype[@@unscopables]` 属性的属性特性：**

- writable: false
- enumerable: false
- configurable: true

🌰：

```js
var keys = [];

with(Array.prototype) {
 keys.push("something");
}

Object.keys(Array.prototype[Symbol.unscopables]);

// Object.keys(Array.prototype[Symbol.unscopables]);
// (13) ['at', 'copyWithin', 'entries', 'fill', 'find', 'findIndex', 'flat', 'flatMap', 'includes', 'keys', 'values', 'findLast', 'findLastIndex']
```

> Array.prototype.length

length 是 Array 的实例属性。返回或设置一个数组中的元素个数。该值是一个无符号 32-bit 整数，并且总是大于数组最高项的下标。

- Writable：如果设置为 false，该属性值将不能被修改。
- Configurable：如果设置为 false，删除或更改任何属性都将会失败。
- Enumerable: 如果设置为 true，属性可以通过迭代器for或for...in进行迭代。

**Array.length 属性的属性特性：**

- writable	true
- enumerable	false
- configurable	false

### 方法

1. 构造函数
2. 静态方法 - Array.isArray()
3. 实例方法
   - valueOf(), toString()
   - push(), pop()
   - shift(), unshift()
   - join()
   - concat()
   - reverse()
   - slice()
   - splice()
   - sort()
   - map()
   - forEach()
   - filter()
   - some(), every()
   - reduce(), reduceRight()
   - indexOf(), lastIndexOf()
   - 链式使用

### 构造函数

Array是 JavaScript 的原生对象，同时也是一个构造函数，可以用它生成新的数组。

```js
var arr = new Array(2);
arr.length // 2
arr // [ empty x 2 ]
```

Array()构造函数的参数2，表示生成一个两个成员的数组，每个位置都是空值。

如果没有使用new关键字，运行结果也是一样的。

```js
var arr = Array(2);
// 等同于
var arr = new Array(2);
```

考虑到语义性，以及与其他构造函数用法保持一致，建议总是加上new。

```js
// 无参数时，返回一个空数组
new Array() // []

// 单个正整数参数，表示返回的新数组的长度
new Array(1) // [ empty ]
new Array(2) // [ empty x 2 ]

// 非正整数的数值作为参数，会报错
new Array(3.2) // RangeError: Invalid array length
new Array(-3) // RangeError: Invalid array length

// 单个非数值（比如字符串、布尔值、对象等）作为参数，
// 则该参数是返回的新数组的成员
new Array('abc') // ['abc']
new Array([1]) // [Array[1]]

// 多参数时，所有参数都是返回的新数组的成员
new Array(1, 2) // [1, 2]
new Array('a', 'b', 'c') // ['a', 'b', 'c']
```

可以看到，Array()作为构造函数，行为很不一致。因此，不建议使用它生成新数组，直接使用数组字面量是更好的做法。

```js
// bad
var arr = new Array(1, 2);

// good
var arr = [1, 2];
```

注意，如果参数是一个正整数，返回数组的成员都是空位。虽然读取的时候返回undefined，但实际上该位置没有任何值。虽然这时可以读取到length属性，但是取不到键名。

```js
var a = new Array(3);
var b = [undefined, undefined, undefined];

a.length // 3
b.length // 3

a[0] // undefined
b[0] // undefined

0 in a // false
0 in b // true
```

上面代码中，a是Array()生成的一个长度为3的空数组，b是一个三个成员都是undefined的数组，这两个数组是不一样的。读取键值的时候，a和b都返回undefined，但是a的键名（成员的序号）都是空的，b的键名是有值的。

### 静态方法

#### Array.isArray()

Array.isArray方法返回一个布尔值，表示参数是否为数组。它可以弥补typeof运算符的不足。

```js
var arr = [1, 2, 3];

typeof arr // "object"
Array.isArray(arr) // true
```

上面代码中，typeof运算符只能显示数组的类型是Object，而Array.isArray方法可以识别数组。

### 实例方法

- valueOf(), toString()

valueOf方法是一个所有对象都拥有的方法，表示对该对象求值。不同对象的valueOf方法不尽一致，数组的valueOf方法返回数组本身。

```js
var arr = [1, 2, 3];
arr.valueOf() // [1, 2, 3]

Array.isArray([1,2,3].valueOf())
true
```

toString方法也是对象的通用方法，数组的toString方法返回数组的字符串形式。

```js
[1,2,3].valueOf()
(3) [1, 2, 3]
[1,2,3].toString()
'1,2,3'

var arr = [1, 2, 3];
arr.toString() // "1,2,3"

var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6"

[1,[2,[3,4],3],4].toString()
'1,2,3,4,3,4'
```

- push(), pop()

push方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```js
var arr = [];

arr.push(1) // 1
arr.push('a') // 2
arr.push(true, {}) // 4
arr // [1, 'a', true, {}]
```

上面代码使用push方法，往数组中添加了四个成员。

pop方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组。

```js
var arr = ['a', 'b', 'c'];

arr.pop() // 'c'
arr // ['a', 'b']
```

对空数组使用pop方法，不会报错，而是返回undefined。

```js
[].pop() // undefined
```

push和pop结合使用，就构成了“后进先出”的栈结构（stack）。

```js
var arr = [];
arr.push(1, 2);
arr.push(3);
arr.pop();
arr // [1, 2]
```

- shift(), unshift()

shift()方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组。

```js
var a = ['a', 'b', 'c'];

a.shift() // 'a'
a // ['b', 'c']
```

上面代码中，使用shift()方法以后，原数组就变了。

shift()方法可以遍历并清空一个数组。

```js
var list = [1, 2, 3, 4];
var item;

while (item = list.shift()) {
  console.log(item);
}

list // []
```

上面代码通过list.shift()方法每次取出一个元素，从而遍历数组。它的前提是数组元素不能是0或任何布尔值等于false的元素，因此这样的遍历不是很可靠。

push()和shift()结合使用，就构成了“先进先出”的队列结构（queue）。

unshift()方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```js
var a = ['a', 'b', 'c'];

a.unshift('x'); // 4
a // ['x', 'a', 'b', 'c']
```

unshift()方法可以接受多个参数，这些参数都会添加到目标数组头部。

```js
var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```

- join()

join()方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分隔。

```js
var a = [1, 2, 3, 4];

a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```

如果数组成员是undefined或null或空位，会被转成空字符串。

```js
[undefined, null].join('#')
// '#'

['a',, 'b'].join('-')
// 'a--b'
```

通过call方法，这个方法也可以用于字符串或类似数组的对象。

```js
Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"

var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```

```js
var a = [1, 2, 3, 4];
a.join()
'1,2,3,4'
a
(4) [1, 2, 3, 4]
```

- concat()

concat方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变。

```js
['hello'].concat(['world'])
// ["hello", "world"]

['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]

[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]

[2].concat({a: 1})
// [2, {a: 1}]
```

除了数组作为参数，concat也接受其他类型的值作为参数，添加到目标数组尾部。

```js
[1, 2, 3].concat(4, 5, 6)
// [1, 2, 3, 4, 5, 6]
```

如果数组成员包括对象，concat方法返回当前数组的一个浅拷贝。所谓“浅拷贝”，指的是新数组拷贝的是对象的引用。

```js
var obj = { a: 1 };
var oldArray = [obj];

var newArray = oldArray.concat();

obj.a = 2;
newArray[0].a // 2
```

上面代码中，原数组包含一个对象，concat方法生成的新数组包含这个对象的引用。所以，改变原对象以后，新数组跟着改变。

- reverse()

reverse方法用于颠倒排列数组元素，返回改变后的数组。注意，该方法将改变原数组。

```js
var a = ['a', 'b', 'c'];

a.reverse() // ["c", "b", "a"]
a // ["c", "b", "a"]
```

- slice()

slice()方法用于提取目标数组的一部分，返回一个新数组，原数组不变。

```js
arr.slice(start, end);
```

它的第一个参数为起始位置（从0开始，会包括在返回的新数组之中），第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。

```js
var a = ['a', 'b', 'c'];

a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice() // ["a", "b", "c"]
```

上面代码中，最后一个例子slice()没有参数，实际上等于返回一个原数组的拷贝。

如果slice()方法的参数是负数，则表示倒数计算的位置。

```js
var a = ['a', 'b', 'c'];
a.slice(-2) // ["b", "c"]
a.slice(-2, -1) // ["b"]
```

上面代码中，-2表示倒数计算的第二个位置，-1表示倒数计算的第一个位置。

如果第一个参数大于等于数组长度，或者第二个参数小于第一个参数，则返回空数组。

```js
var a = ['a', 'b', 'c'];
a.slice(4) // []
a.slice(2, 1) // []
```

slice()方法的一个重要应用，是将类似数组的对象转为真正的数组。

```js
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```

上面代码的参数都不是数组，但是通过call方法，在它们上面调用slice()方法，就可以把它们转为真正的数组。

- splice()

splice()方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。

```js
arr.splice(start, count, addElement1, addElement2, ...);
```

splice的第一个参数是删除的起始位置（从0开始），第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2) // ["e", "f"]
a // ["a", "b", "c", "d"]
```

上面代码从原数组4号位置，删除了两个数组成员。

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2, 1, 2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
```

上面代码除了删除成员，还插入了两个新成员。

起始位置如果是负数，就表示从倒数位置开始删除。

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(-4, 2) // ["c", "d"]
```

上面代码表示，从倒数第四个位置c开始删除两个成员。

如果只是单纯地插入元素，splice方法的第二个参数可以设为0。

```js
var a = [1, 1, 1];

a.splice(1, 0, 2) // []
a // [1, 2, 1, 1]
```

如果只提供第一个参数，等同于将原数组在指定位置拆分成两个数组。

```js
var a = [1, 2, 3, 4];
a.splice(2) // [3, 4]
a // [1, 2]
```

- sort()

sort方法对数组成员进行排序，默认是按照字典顺序排序。排序后，原数组将被改变。

```js
['d', 'c', 'b', 'a'].sort()
// ['a', 'b', 'c', 'd']

[4, 3, 2, 1].sort()
// [1, 2, 3, 4]

[11, 101].sort()
// [101, 11]

[10111, 1101, 111].sort()
// [10111, 1101, 111]
```

上面代码的最后两个例子，需要特殊注意。sort()方法不是按照大小排序，而是按照字典顺序。也就是说，数值会被先转成字符串，再按照字典顺序进行比较，所以101排在11的前面。

如果想让sort方法按照自定义方式排序，可以传入一个函数作为参数。

```js
[10111, 1101, 111].sort(function (a, b) {
  return a - b;
})
// [111, 1101, 10111]
```

上面代码中，sort的参数函数本身接受两个参数，表示进行比较的两个数组成员。如果该函数的返回值大于0，表示第一个成员排在第二个成员后面；其他情况下，都是第一个元素排在第二个元素前面。

```js
[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28  }
].sort(function (o1, o2) {
  return o1.age - o2.age;
})
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

注意，自定义的排序函数应该返回数值，否则不同的浏览器可能有不同的实现，不能保证结果都一致。

```js
// bad
[1, 4, 2, 6, 0, 6, 2, 6].sort((a, b) => a > b)

// good
[1, 4, 2, 6, 0, 6, 2, 6].sort((a, b) => a - b)
```

上面代码中，前一种排序算法返回的是布尔值，这是不推荐使用的。后一种是数值，才是更好的写法。

- map()

map()方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回。

```js
var numbers = [1, 2, 3];

numbers.map(function (n) {
  return n + 1;
});
// [2, 3, 4]

numbers
// [1, 2, 3]
```

上面代码中，numbers数组的所有成员依次执行参数函数，运行结果组成一个新数组返回，原数组没有变化。

map()方法接受一个函数作为参数。该函数调用时，map()方法向它传入三个参数：当前成员、当前位置和数组本身。

```js
[1, 2, 3].map(function(elem, index, arr) {
  return elem * index;
});
// [0, 2, 6]
```

上面代码中，map()方法的回调函数有三个参数，elem为当前成员的值，index为当前成员的位置，arr为原数组（[1, 2, 3]）。

map()方法还可以接受第二个参数，用来绑定回调函数内部的this变量（详见《this 变量》一章）。

```js
var arr = ['a', 'b', 'c'];

[1, 2].map(function (e) {
  return this[e];
}, arr)
// ['b', 'c']
```

上面代码通过map()方法的第二个参数，将回调函数内部的this对象，指向arr数组。

如果数组有空位，map()方法的回调函数在这个位置不会执行，会跳过数组的空位。

```js
var f = function (n) { return 'a' };

[1, undefined, 2].map(f) // ["a", "a", "a"]
[1, null, 2].map(f) // ["a", "a", "a"]
[1, , 2].map(f) // ["a", , "a"]
```

上面代码中，map()方法不会跳过undefined和null，但是会跳过空位。

- forEach()

forEach()方法与map()方法很相似，也是对数组的所有成员依次执行参数函数。但是，forEach()方法不返回值，只用来操作数据。这就是说，如果数组遍历的目的是为了得到返回值，那么使用map()方法，否则使用forEach()方法。

forEach()的用法与map()方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。

```js
function log(element, index, array) {
  console.log('[' + index + '] = ' + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

上面代码中，forEach()遍历数组不是为了得到返回值，而是为了在屏幕输出内容，所以不必使用map()方法。

forEach()方法也可以接受第二个参数，绑定参数函数的this变量。

```js
var out = [];

[1, 2, 3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out // [1, 4, 9]
```

上面代码中，空数组out是forEach()方法的第二个参数，结果，回调函数内部的this关键字就指向out。

注意，forEach()方法无法中断执行，总是会将所有成员遍历完。如果希望符合某种条件时，就中断遍历，要使用for循环。

```js
var arr = [1, 2, 3];

for (var i = 0; i < arr.length; i++) {
  if (arr[i] === 2) break;
  console.log(arr[i]);
}
// 1
```

上面代码中，执行到数组的第二个成员时，就会中断执行。forEach()方法做不到这一点。

forEach()方法也会跳过数组的空位。

```js
var log = function (n) {
  console.log(n + 1);
};

[1, undefined, 2].forEach(log)
// 2
// NaN
// 3

[1, null, 2].forEach(log)
// 2
// 1
// 3

[1, , 2].forEach(log)
// 2
// 3
```

上面代码中，forEach()方法不会跳过undefined和null，但会跳过空位。

- filter()

filter()方法用于过滤数组成员，满足条件的成员组成一个新数组返回。

它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。

```js
[1, 2, 3, 4, 5].filter(function (elem) {
  return (elem > 3);
})
// [4, 5]
```

上面代码将大于3的数组成员，作为一个新数组返回。

```js
var arr = [0, 1, 'a', false];

arr.filter(Boolean)
// [1, "a"]
```

上面代码中，filter()方法返回数组arr里面所有布尔值为true的成员。

filter()方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。

```js
[1, 2, 3, 4, 5].filter(function (elem, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]
```

上面代码返回偶数位置的成员组成的新数组。

filter()方法还可以接受第二个参数，用来绑定参数函数内部的this变量。

```js
var obj = { MAX: 3 };
var myFilter = function (item) {
  if (item > this.MAX) return true;
};

var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj) // [8, 4, 9]
```

上面代码中，过滤器myFilter()内部有this变量，它可以被filter()方法的第二个参数obj绑定，返回大于3的成员。

- some(), every()

这两个方法类似“断言”（assert），返回一个布尔值，表示判断数组成员是否符合某种条件。

它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员、当前位置和整个数组，然后返回一个布尔值。

some方法是只要一个成员的返回值是true，则整个some方法的返回值就是true，否则返回false。

```js
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true
```

上面代码中，如果数组arr有一个成员大于等于3，some方法就返回true。

every方法是所有成员的返回值都是true，整个every方法才返回true，否则返回false。

```js
var arr = [1, 2, 3, 4, 5];
arr.every(function (elem, index, arr) {
  return elem >= 3;
});
// false
```

上面代码中，数组arr并非所有成员大于等于3，所以返回false。

注意，对于空数组，some方法返回false，every方法返回true，回调函数都不会执行。

```js
function isEven(x) { return x % 2 === 0 }

[].some(isEven) // false
[].every(isEven) // true
```

some和every方法还可以接受第二个参数，用来绑定参数函数内部的this变量。

- reduce(), reduceRight()

reduce()方法和reduceRight()方法依次处理数组的每个成员，最终累计为一个值。它们的差别是，reduce()是从左到右处理（从第一个成员到最后一个成员），reduceRight()则是从右到左（从最后一个成员到第一个成员），其他完全一样。

由于这两个方法会遍历数组，所以实际上可以用来做一些遍历相关的操作。比如，找出字符长度最长的数组成员。

```js
function findLongest(entries) {
  return entries.reduce(function (longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}

findLongest(['aaa', 'bb', 'c']) // "aaa"
```

上面代码中，reduce()的参数函数会将字符长度较长的那个数组成员，作为累积值。这导致遍历所有成员之后，累积值就是字符长度最长的那个成员。

- indexOf(), lastIndexOf()

indexOf方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。

```js
var a = ['a', 'b', 'c'];

a.indexOf('b') // 1
a.indexOf('y') // -1
```

indexOf方法还可以接受第二个参数，表示搜索的开始位置。

```js
['a', 'b', 'c'].indexOf('a', 1) // -1
```

上面代码从1号位置开始搜索字符a，结果为-1，表示没有搜索到。

lastIndexOf方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。

```js
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
a.lastIndexOf(7) // -1
```

注意，这两个方法不能用来搜索NaN的位置，即它们无法确定数组成员是否包含NaN。

```js
[NaN].indexOf(NaN) // -1
[NaN].lastIndexOf(NaN) // -1
```

这是因为这两个方法内部，使用严格相等运算符（===）进行比较，而NaN是唯一一个不等于自身的值。

### 链式使用

```js
var users = [
  {name: 'tom', email: 'tom@example.com'},
  {name: 'peter', email: 'peter@example.com'}
];

users
.map(function (user) {
  return user.email;
})
.filter(function (email) {
  return /^t/.test(email);
})
.forEach(function (email) {
  console.log(email);
});
// "tom@example.com"
```

上面代码中，先产生一个所有 Email 地址组成的数组，然后再过滤出以t开头的 Email 地址，最后将它打印出来。

## Array

与其他编程语言中的数组一样，Array 对象支持在单个变量名下存储多个元素，并具有执行常见数组操作的成员。

### 描述

在 JavaScript 中，数组不是基本类型，而是具有以下核心特征的 Array 对象：

- JavaScript 数组是可调整大小的，并且可以包含不同的数据类型。（当不需要这些特征时，可以使用类型化数组。）
- JavaScript 数组不是关联数组，因此，不能使用任意字符串作为索引访问数组元素，但必须使用非负整数（或它们各自的字符串形式）作为索引访问。
- JavaScript 数组的索引从 0 开始：数组的第一个元素在索引 0 处，第二个在索引 1 处，以此类推，最后一个元素是数组的 length 属性减去 1 的值。
- JavaScript 数组复制操作创建浅拷贝。（所有 JavaScript 对象的标准内置复制操作都会创建浅拷贝，而不是深拷贝）。

### 数组下标

Array 对象不能使用任意字符串作为元素索引（如关联数组），必须使用非负整数（或它们的字符串形式）。通过非整数设置或访问不会设置或从数组列表本身检索元素， 但会设置或访问与该数组的对象属性集合相关的变量。数组的对象属性和数组元素列表是分开的，数组的遍历和修改操作不能应用于这些命名属性。

数组元素是对象属性，就像 toString 是属性一样（具体来说，toString() 是一种方法）。然而，尝试按以下方式访问数组的元素会抛出语法错误，因为属性名无效：

`console.log(arr.0); // a syntax error`

JavaScript 语法要求使用方括号表示法而不是点号表示法来访问以数字开头的属性。也可以用引号包裹数组下标（例如，years['2'] 而不是 years[2]），尽管通常没有必要。

JavaScript 引擎通过隐式的 toString，将 years[2] 中的 2 强制转换为字符串。因此，'2' 和 '02' 将指向 years 对象上的两个不同的槽位，下面的例子可能是 true：

`console.log(years['2'] !== years['02']);`

只有 years['2'] 是一个实际的数组索引。years['02'] 是一个在数组迭代中不会被访问的任意字符串属性。

### 数组方法和空槽

稀疏数组中的空槽在数组方法之间的行为不一致。通常，旧方法会跳过空槽，而新方法将它们视为 undefined。

在遍历多个元素的方法中，下面的方法在访问索引之前执行 in 检查，并且不将空槽与 undefined 合并：

- concat()
- copyWithin()
- every()
- filter()
- flat()
- flatMap()
- forEach()
- indexOf()
- lastIndexOf()
- map()
- reduce()
- reduceRight()
- reverse()
- slice()
- some()
- sort()
- splice()

这些方法将空槽视为 undefined：

- entries()
- fill()
- find()
- findIndex()
- findLast()
- findLastIndex()
- group()
- groupToMap()
- includes()
- join()
- keys()
- toLocaleString()
- values()

### 复制方法和修改方法

有些方法不改变调用该方法的现有数组，而是返回一个新数组。它们首先访问 this.constructor[Symbol.species] 来确定用于新数组的构造函数。然后用元素填充新构造的数组。复制总是触发浅拷贝——该方法从不复制初始创建数组以外的任何内容。原数组的元素按如下方式复制到新数组中：

- 对象：对象引用被复制到新数组中。原数组和新数组都引用同一个对象。也就是说，如果一个被引用的对象被修改，新数组和原数组都可以看到更改。
- 基本类型，如字符串，数字和布尔值（不是 String、Number 和 Boolean 对象）：它们的值被复制到新数组中。

其他方法会改变调用该方法的数组，在这种情况下，它们的返回值根据方法的不同而不同：有时是对相同数组的引用，有时是新数组的长度。

以下方法使用 @@species 创建新数组：

- concat()
- filter()
- flat()
- flatMap()
- map()
- slice()
- splice() (构造返回的已删除元素数组）

注意，group() (en-US) 和 groupToMap() (en-US) 不使用 @@species 为每个组条目创建新数组，而是始终使用普通的 Array 构造函数。从概念上讲，它们也不是复制方法。

以下方法可以对原数组进行修改：

- copyWithin()
- fill()
- pop()
- push()
- reverse()
- shift()
- sort()
- splice()
- unshift()

### 通用数组方法

数组方法总是通用的——它们不访问数组对象的任何内部数据。它们只通过 length 属性和索引访问数组元素。这意味着它们也可以在类数组对象上调用。

```
const arrayLike = {
  0: "a",
  1: "b",
  length: 2,
};
console.log(Array.prototype.join.call(arrayLike, "+")); // 'a+b'
```

### 长度属性的规范化

length 属性被转换为一个数字，被截断为一个整数，然后固定为 0 到 253 - 1 之间的范围。NaN 变成 0，所以即使 length 没有出现或 undefined，它也会表现得好像它的值是 0。

`Array.prototype.flat.call({}); // []`

一些数组方法会设置数组对象的 length 属性。它们总是在规范化后设置值，因此 length 总是以整数结尾。

```
const a = { length: 0.7 };
Array.prototype.push.call(a);
console.log(a.length); // 0
```

### 类数组对象

术语类数组对象指的是在上面描述的 length 转换过程中不抛出的任何对象。在实践中，这样的对象应该实际具有 length 属性，并且索引元素的范围在 0 到 length - 1 之间。（如果它没有所有的索引，它将在功能上等同于稀疏数组。）

许多 DOM 对象都是类数组对象——例如 NodeList 和 HTMLCollection。arguments 对象也是类数组对象。你可以在它们上调用数组方法，即使它们本身没有这些方法。

```
function f() {
  console.log(Array.prototype.join.call(arguments, "+"));
}

f("a", "b"); // 'a+b'
```

### 构造函数

Array()

创建一个新的 Array 对象。

### 静态属性

get Array[@@species]

返回 Array 构造函数。

### 静态方法

`Array.from()`
从数组类对象或可迭代对象创建一个新的 Array 实例。

`Array.isArray()`
如果参数是数组则返回 true ，否则返回 false 。

`Array.of()`
创建一个新的 Array 实例，具有可变数量的参数，而不管参数的数量或类型。

### 实例属性

`Array.prototype.length`
反映数组中元素的数量。

`Array.prototype[@@unscopables]`
包含 ES2015 版本之前 ECMAScript 标准中没有包含的属性名，在使用 with 绑定语句时会被忽略。

### 实例方法

Array.prototype.at()
返回给定索引处的数组元素。接受从最后一项往回计算的负整数。

Array.prototype.concat()
返回一个新数组，该数组由被调用的数组与其它数组或值连接形成。

Array.prototype.copyWithin()
在数组内复制数组元素序列。

Array.prototype.entries()
返回一个新的数组迭代器对象，其中包含数组中每个索引的键/值对。

Array.prototype.every()
如果调用数组中的每个元素都满足测试函数，则返回 true。

Array.prototype.fill()
用静态值填充数组中从开始索引到结束索引的所有元素。

Array.prototype.filter()
返回一个新数组，其中包含调用所提供的筛选函数返回为 true 的所有数组元素。

Array.prototype.find()
返回数组中满足提供的测试函数的第一个元素的值，如果没有找到合适的元素，则返回 undefined。

Array.prototype.findIndex()
返回数组中满足提供的测试函数的第一个元素的索引，如果没有找到合适的元素，则返回 -1。

Array.prototype.findLast()
返回数组中满足提供的测试函数的最后一个元素的值，如果没有找到合适的元素，则返回 undefined。

Array.prototype.findLastIndex()
返回数组中满足所提供测试函数的最后一个元素的索引，如果没有找到合适的元素，则返回 -1。

Array.prototype.flat()
返回一个新数组，所有子数组元素递归地连接到其中，直到指定的深度。

Array.prototype.flatMap()
对调用数组的每个元素调用给定的回调函数，然后将结果平展一层，返回一个新数组。

Array.prototype.forEach()
对调用数组中的每个元素调用函数。

Array.prototype.group() (en-US) 实验性
根据测试函数返回的字符串，将数组的元素分组到一个对象中。

Array.prototype.groupToMap() (en-US) 实验性
根据测试函数返回的值，将数组的元素分组到 Map 中。

Array.prototype.includes()
确定调用数组是否包含一个值，根据情况返回 true 或 false。

Array.prototype.indexOf()
返回在调用数组中可以找到给定元素的第一个（最小）索引。

Array.prototype.join()
将数组的所有元素连接为字符串。

Array.prototype.keys()
返回一个新的数组迭代器，其中包含调用数组中每个索引的键。

Array.prototype.lastIndexOf()
返回在调用数组中可以找到给定元素的最后一个（最大）索引，如果找不到则返回 -1。

Array.prototype.map()
返回一个新数组，其中包含对调用数组中的每个元素调用函数的结果。

Array.prototype.pop()
从数组中移除最后一个元素并返回该元素。

Array.prototype.push()
在数组末尾添加一个或多个元素，并返回数组新的 length。

Array.prototype.reduce()
对数组的每个元素（从左到右）执行用户提供的 “reducer” 回调函数，将其简化为单个值。

Array.prototype.reduceRight()
对数组的每个元素（从右到左）执行用户提供的 “reducer” 回调函数，将其简化为单个值。

Array.prototype.reverse()
反转数组中元素的顺序。（前面变成后面，后面变成前面。）

Array.prototype.shift()
从数组中移除第一个元素并返回该元素。

Array.prototype.slice()
提取调用数组的一部分并返回一个新数组。

Array.prototype.some()
如果调用数组中至少有一个元素满足提供的测试函数，则返回 true。

Array.prototype.sort()
对数组的元素进行排序并返回该数组。

Array.prototype.splice()
从数组中添加和/或删除元素。

Array.prototype.toLocaleString()
返回一个表示调用数组及其元素的本地化字符串。重写 Object.prototype.toLocaleString() 方法。

Array.prototype.toString()
返回一个表示调用数组及其元素的字符串。重写 Object.prototype.toString() 方法。

Array.prototype.unshift()
在数组的前面添加一个或多个元素，并返回数组新的 length。

Array.prototype.values()
返回一个新的数组迭代器对象，该对象包含数组中每个索引的值。

Array.prototype[@@iterator]()
默认情况下，该方法为 values() 方法的别名。

### 遍历数组

下面的例子使用 for...of 循环遍历 fruits 数组，将每一个元素打印到控制台。

```
const fruits = ['Apple', 'Mango', 'Cherry'];
for (const fruit of fruits) {
  console.log(fruit);
}
// Apple
// Mango
// Cherry
```

### 对数组中的每个元素调用函数

下面的例子使用 forEach() 方法在 fruits 数组中的每个元素上调用一个函数；该函数将每个元素以及元素的索引号打印到控制台。

```
const fruits = ['Apple', 'Mango', 'Cherry'];
fruits.forEach((item, index, array) => {
  console.log(item, index);
});
// Apple 0
// Mango 1
// Cherry 2
```

### 复制数组

下面的例子展示了从现有的 fruits 数组创建新数组的三种方法：首先使用展开语法，然后使用 from() 方法，然后使用 slice() 方法。

```
const fruits = ['Strawberry', 'Mango'];

// Create a copy using spread syntax.
const fruitsCopy = [...fruits];
// ["Strawberry", "Mango"]

// Create a copy using the from() method.
const fruitsCopy2 = Array.from(fruits);
// ["Strawberry", "Mango"]

// Create a copy using the slice() method.
const fruitsCopy3 = fruits.slice();
// ["Strawberry", "Mango"]
```







