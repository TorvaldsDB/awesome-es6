# Iterators

## [Iterators and generators - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)

```js
function* makeIterator() {
    yield 1;
    yield 2;
}

const it = makeIterator();

for (const itItem of it) {
    console.log(itItem);
}

console.log(it[Symbol.iterator]() === it) // true;

// This example show us generator(iterator) is iterable object,
// which has the @@iterator method return the it (itself),
// and consequently, the it object can iterate only _once_.

// If we change it's @@iterator method to a function/generator
// which returns a new iterator/generator object, (it)
// can iterate many times

it[Symbol.iterator] = function* () {
  yield 2;
  yield 1;
};
```

[Can I yield from an inner function?](https://stackoverflow.com/questions/29313604/can-i-yield-from-an-inner-function)

No, you can't use [`yield`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield) inside of the inner function. But in your case you don't need it. You can always use [`for-of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop instead of [`forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) method. It will look much prettier, and you can use [`continue`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue), [`break`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break), [`yield`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield) inside it:

```js
var trivialGenerator = function *(array) {
  for (var item of array) {
    // some item manipulation
    yield item;
  }
}
```

You can use `for-of` if you have some manipulations with item inside it. Otherwise you absolutely don't need to create this generator, as `array` has [`iterator` interface](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/The_Iterator_protocol) natively.

- wrong way use `forEach`

下面代码也会产生句法错误，因为forEach方法的参数是一个普通函数，但是在里面使用了yield表达式（这个函数里面还使用了yield*表达式，详细介绍见后文）。一种修改方法是改用for循环。

```js
var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  a.forEach(function (item) {
    if (typeof item !== 'number') {
      yield* flat(item);
    } else {
      yield item;
    }
  });
};

for (var f of flat(arr)){
  console.log(f);
}
```

使用 `for` 代替

```js
var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  var length = a.length;
  for (var i = 0; i < length; i++) {
    var item = a[i];
    if (typeof item !== 'number') {
      yield* flat(item);
    } else {
      yield item;
    }
  }
};

for (var f of flat(arr)) {
  console.log(f);
}
// 1, 2, 3, 4, 5, 6
```

使用 `for...of` 代替

```js
var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  for(const item of a) {
    if (typeof item !== 'number') {
      yield* flat(item);
    } else {
      yield item;
    }
  };
};

for (var f of flat(arr)){
  console.log(f);
}

// 1, 2, 3, 4, 5, 6
```

## next 方法的参数

`yield`表达式本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当作**上一个**`yield`表达式的返回值。

**next携带的参数, 作为的是上一个yield表达式的返回值**

```js
function* f() {
  console.log('start')
	yield 1
  const reset = yield 2
  const reset1 = yield 3
  if(reset) {
   	if (reset1) {
    	yield 4
    } else {
    	yield 5
    }
  } else {
  	if (reset1) {
    	yield 6
    } else {
    	yield 7
    }
  }
}

var g = f(); // do nothing

console.log(g.next())
/*
"start"
{
  done: false,
  value: 1
}
*/
console.log(g.next())
/*
{
  done: false,
  value: 2
}
*/
console.log(g.next(true))
/*
{
  done: false,
  value: 3
}
*/
console.log(g.next(true))
/*
{
  done: false,
  value: 4
}

*/
console.log(g.next())
/*
{
  done: true,
  value: undefined
}
*/
```

```js
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

**注意**，*由于next方法的参数表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的。V8 引擎直接忽略第一次使用next方法时的参数，只有从第二次使用next方法开始，参数才是有效的。从语义上讲，**第一个next方法用来启动遍历器对象，所以不用带有参数**。*

```js
function* dataConsumer() {
  console.log('Started');
  console.log(`1. ${yield}`);
  console.log(`2. ${yield}`);
  return 'result';
}

let genObj = dataConsumer();
genObj.next();
// Started
genObj.next('a')
// 1. a
genObj.next('b')
// 2. b
```

如果想要第一次调用next方法时，就能够输入值，可以在 Generator 函数外面再包一层。

```js
function wrapper(generatorFunction) {
  return function (...args) {
    let generatorObject = generatorFunction(...args);
    generatorObject.next();
    return generatorObject;
  };
}

const wrapped = wrapper(function* () {
  console.log(`First input: ${yield}`);
  return 'DONE';
});

wrapped().next('hello!')
// First input: hello!
```

除了for...of循环以外，扩展运算符（...）、解构赋值和Array.from方法内部调用的，都是遍历器接口。这意味着，它们都可以将 Generator 函数返回的 Iterator 对象，作为参数。

```js
function* numbers () {
  yield 1
  yield 2
  return 3
  yield 4
}

// 扩展运算符
[...numbers()] // [1, 2]

// Array.from 方法
Array.from(numbers()) // [1, 2]

// 解构赋值
let [x, y] = numbers();
x // 1
y // 2

// for...of 循环
for (let n of numbers()) {
  console.log(n)
}
// 1
// 2
```