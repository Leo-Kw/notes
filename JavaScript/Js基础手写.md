# 基础手写

手写js的一些基础方法

## instanceof

- 如果 `target` 为基本数据类型直接返回 `false` 。
- 通过一个循环 `while` 判断 `Fn.prototype` 是否在 `target` 的隐式原型链上。

```javascript
const _instanceof = (target, Fn) => {
  if (
    (typeof target !== "object" && typeof target !== "function") ||
    target === null
  )
    return false;
  let proto = target.__proto__;
  while (true) {
    if (proto === null) return false;
    if (proto === Fn.prototype) return true;
    proto = proto.__proto__;
  }
};

function A() {}
function B() {}
const a = new A();
console.log(_instanceof(a, A)); // true
console.log(_instanceof(a, B)); // false
console.log(_instanceof(1, A)); // false

```

## Array.prototype.forEach

- `forEach` 中的 `exc` 接受三个参数，分别是: 元素值、元素下标和原数组。

```javascript
Array.prototype._forEach = function (exc) {
  if (typeof exc !== "function") {
    throw new Error(`${exc} is not a function`);
  }
  if (!Array.isArray(this)) {
    throw new Error(`${this} is not a Array`);
  }
  let arr = this;
  for (let i = 0; i < arr.length; i++) {
    exc(arr[i], i, arr);
  }
};

var arr = [1, 2, 4, 5];
arr._forEach((num, index) => {
  return (arr[index] = num * 2);
});
console.log(arr); // [ 2, 4, 8, 10 ]
```

## Array.prototype.map

- `map` 中的 `exc` 接受三个参数，分别是: 元素值、元素下标和原数组。
- `map` 返回的是一个新的数组，地址不一样。
- `forEach` 可参照上面进行实现。

```javascript
Array.prototype._map = function (exc) {
  const result = [];
  this.forEach((item, index, arr) => {
    result[index] = exc(item, index, arr);
  });
  return result;
};

const a = new Array(2).fill(2);
console.log(a._map((item, index, arr) => item * index + 1)); // [ 1, 3 ]
```

## Array.prototype.filter

- `filter` 中的 `exc` 接受三个参数，与 `map` 一致，主要实现的是数组的过滤功能，会根据 `exc` 函数的返回值来判断是否“留下”该值。
- `filter` 返回的是一个新的数组，地址不一致。

```javascript
Array.prototype._filter = function (exc) {
  const result = [];
  this._forEach((item, index, arr) => {
    exc(item, index, arr) && result.push(item);
  });
  return result;
};

const b = [1, 3, 4, 5, 6, 2, 5, 1, 8, 20];
console.log(b._filter((item) => item % 2 === 0)); // [ 4, 6, 2, 8, 20 ]
```

## Array.prototype.reduce

- `reduce` 接受两个参数，第一个为 `exc` 函数，第二个为初始值，如果不传默认为 0。
- `reduce` 最终会返回一个值，当然不一定是 `Number` 类型的，取决于你是怎么计算的，每次的计算结果都会作为下次 `exc` 中的第一个参数。

```javascript
Array.prototype._reduce = function (exc, initial = 0) {
  let result = initial;
  this._forEach((item, index, arr) => {
    result = exc(result, item, index, arr);
  });
  return result;
};

const b = [1, 10];
console.log(b._reduce((pre, cur) => pre + cur, 2)); // 13
```

## Object.create

**[MDN](*<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create>*)** `Object.create()` 方法用于创建一个新对象，使用现有的对象来作为新创建对象的原型（`prototype`）。

```javascript
Object.prototype._create = function (proto) {
  const Fn = function () {};
  Fn.prototype = proto;
  return new Fn();
};

function A() {}
const obj = Object.create(A);
const obj2 = Object._create(A);
console.log(obj.__proto__ === A); // true
console.log(obj.__proto__ === A); // true
```

## Function.prototype.call

- `call()` 方法使用一个指定的 `this` 值和单独给出的一个或多个参数来调用一个函数。
- `call` 与  `apply` 区别在于 `apply` 的第二个参数是数组。

```javascript
Function.prototype._call = function (ctx, ...args) {
  // 如果不为空，则需要进行对象包装
  const o = ctx === undefined ? window : Object(ctx);
  // 给 ctx 添加独一无二的属性
  const key = Symbol();
  o[key] = this;
  // 执行函数，得到返回结果
  const result = o[key](...args);
  // 删除该属性
  delete o[key];
  return result;
};

const obj = {
  name: "11",
  fun() {
    console.log(this.name);
  },
};

const obj2 = { name: "22" };
obj.fun(); // 11
obj.fun.call(obj2); // 22
obj.fun._call(obj2); // 22

```

## Function.prototype.bind

`bind()` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

```javascript
Function.prototype._bind = function (ctx, ...args) {
  if (typeof this !== "function") return console.error("Error");
  // 如果不为空，则需要进行对象包装
  const o = ctx === undefined ? window : Object(ctx);
  // 给 ctx 添加独一无二的属性
  const key = Symbol();
  o[key] = this;
  return (bindFn = (...rest) => {
    const result = o[key](...args, ...rest);
    delete o[key];
    return result;
  });
};

const obj = {
  name: "11",
  fun(value) {
    console.log("name:", this.name, ", value:", value);
  },
};
const obj2 = { name: "22" };
const fn2 = obj.fun._bind(obj2, 1);

fn2(2);

```
