# Set

**Set** 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。Set 本身是一个构造函数，用来生成 Set 数据结构。

## 语法

```javascript
new Set( [iterable] )
```

### 参数

| 参数       | 类型         | 描述                                                         |
| ---------- | ------------ | ------------------------------------------------------------ |
| `iterable` | `Array` 类型 | 如果传递一个可迭代对象，它的所有元素将被添加到新的 `Set` 中。如果不指定此参数或其值为 `null`，则新的 `Set` 为空。 |

### 描述 

向 `Set` 加入值时不会发生类型转换，所以 `5` 和 `'5'` 是两个不同的值。`Set` 内部判断两个值是否相同时使用的算法叫做 “Same-value equality”，它类似于精准相等运算符（`===`），主要的区别是 `NaN` 等于自身，而精准相等运算符认为 `NaN` 不等于自身。

## `Set` 实例的属性和方法

### 属性

| 属性                        | 描述                          |
| --------------------------- | ----------------------------- |
| `Set.prototype.constructor` | 构造函数，默认是 `Set` 函数。 |
| `Set.prototype.size`        | 返回 `Set` 实例的成员总数     |

### 方法

`Set` 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。

#### 操作方法

| 方法                          | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| `Set.prototype.add(value)`    | 用来向一个 `Set` 对象的末尾添加一个指定的值。                |
| `Set.prototype.delete(value)` | 可以从一个 `Set` 对象中删除指定的元素。                      |
| `Set.prototype.has(value)`    | 返回一个布尔值来指示对应的值 `value` 是否存在 `Set` 对象中。 |
| `Set.prototype.clear()`       | 清空一个 `Set` 对象中的所有元素。                            |

#### 遍历方法

| 方法                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `Set.prototype.keys()`    | 与 `values()` 方法相同，返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。 |
| `Set.prototype.values()`  | 返回一个 `Iterator` 对象，这个对象以插入 `Set` 对象的顺序包含了原 `Set` 对象里的每个元素。 |
| `Set.prototype.entries()` | 返回一个新的迭代器对象，这个对象的元素是类似 `[value, value]` 形式的数组，`value` 是集合对象爱你个中的每个元素，迭代器对象元素的顺序即集合对象爱你个中元素插入的顺序。由于集合对象不像 `Map` 对象那样拥有 `key` ，然而，为了与 `Map` 对象的API形式保持一致，故使得每一个 `entry` 的 `key` 和 `value` 都拥有相同的值，因而最终返回一个 `[value, value]` 形式的数组。 |
| `Set.prototype.forEach()` | 根据集合中元素的顺序，对每个元素都执行提供的回调函数一次。   |

需要特别指出的是，`Set` 的遍历顺序就是插入顺序。这个特性有时非常有用，比如使用 `Set` 保存一个回调函数列表，调用时就能保证按照添加顺序调用。

`keys` 方法、`values` 方法、`entries` 方法返回的都是遍历器对象爱你个。由于 `Set` 结构没有键名，只有键值（或者说键名和键值是同一个值），所以 `keys` 方法和 `values` 方法的行为完全一致。

##### Set.prototype.keys()

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
    console.log(item);
}
// red
// green
// blue
```

##### Set.prototype.values() 

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.values()) {
    console.log(item)
}

// red
// green
// blue
```

##### Set.prototype.entries() 

`entries` 方法返回的遍历器同时包括键名和键值，所以每次输出一个数据，其两个成员完全相等。

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.entries()) {
    console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

`Set` 结构的实例默认可遍历，其默认遍历器生成函数就是它的 `values` 方法。

```javascript
Set.prototype[Symbol.iterator] === Set.prototype.values
// true
```

这意味着，可以省略 `values` 方法，直接用 `for...of` 循环遍历 `Set`。

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
    console.log(x);
}
// red
// green
// blue
```

##### `Set.prototype.forEach()`

`Set` 结构实例的 `forEach` 方法用于对每个成员执行某种操作，没有返回值。

```javascript
let set = new Set([1, 2, 3]);

set.forEach((value, key) => console.log(value * 2))
// 2
// 4
// 6
```

上面代码说明，`forEach` 方法的参数就是一个处理函数。该函数的参数依次为键值、键名、集合本身（上例省略了该参数）。另外，`forEach` 方法还可以有第二个参数，表示绑定的 `this` 对象。

## 示例

### 标准示例

```javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s){
    console.log(i);
}
// 2 3 5 4
```

### `Set` 内部判断机制

#### `Set` 内部 `NaN` 相等

下面的代码向 `Set` 实例添加了两个 `NaN` ，但是实际上只能添加一个。这表明，在 `Set` 内部，两个 `NaN` 是相等的。

```javascript
let set = new Set();
let a = NaN;
let b = NaN;
set.add(a);
set.add(b);

set	// Set{NaN}
```

#### `Set` 内部对象不相等

下面的代码表示，由于两个空对象不是精准相等，所以它们被视为两个值。

```javascript
let set = new Set();

set.add({})
set.size;	// 1

set.add({})
set.size;	// 2
```

### `Set` 结构转为数组类型

`Array.from` 方法可以将 `Set` 结构转为数组

```javascript
const items = new Set([1, 2, 3, 4, 5]);
const array =Array.from(items);
```

这就提供了一种去除数组重复元素的方法

```javascript
function dedupe(array){
    return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3])	// [1, 2, 3]
```

### 数组去重

```javascript
// [...new Set(array)]

const array = [1, 2, 2, 3, 4, 4, 5, 5, 5];

console.log([...new Set(array)]);
// [1, 2, 3, 4, 5]
```

### 扩展运算的应用

扩展运算符 `...` 内部使用 `for...of` 循环，所以也可以用于 `Set` 结构。

```javascript
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```

### 并集、交集和差集的实现

使用 `Set` 可以很容易地实现并集（Union）、交集（Interest）和差集（Difference）。

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let interest = new Set([...a].filter(x => b.has(x)));
// Set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

### `Set` 结构的改变

如果想在遍历操作中同步改变原来的 `Set` 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 `Set` 结构映射出一个新的结构，然后赋值给原来的 `Set` 结构；另一种是利用 `Array.from` 方法。

```javascript
// Method A
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set 的值是2, 4, 6

// Method B
let set = new Set([1, 2, 3]);
set new Set(Array.from(set, val => val * 2));
// set 的值是2, 4, 6
```
















