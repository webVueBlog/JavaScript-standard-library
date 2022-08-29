# 《前端进阶 JavaScript 标准库》 - 前端进阶必看的JavaScript 标准库

<img width="300px" src="https://user-images.githubusercontent.com/59645426/187019807-7785b3c9-9931-4bca-a28f-5e508a4c9839.jpg"/>

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










