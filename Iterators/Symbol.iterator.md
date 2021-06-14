# [Symbol.iterator - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator)

|Property attributes of Symbol.iterator|--|
|--|:--|
|Writable|no|
|Enumerable|no|
|Configurable|no|

## User-defined iterables

We can make our own iterables like this:

```js
var myIterable = {}
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
[...myIterable] // [1, 2, 3]
```

Or iterables can be defined directly inside a class or object using a computed property:

the star`*` before `*[Symbol.iterator]` is required. It means the method is a Generator function.
```js
class Foo {
  *[Symbol.iterator] () {
    yield 1;
    yield 2;
    yield 3;
  }
}

const someObj = {
  *[Symbol.iterator] () {
    yield 'a';
    yield 'b';
  }
}

console.log(...new Foo); // 1, 2, 3
console.log(...someObj); // 'a', 'b'
```