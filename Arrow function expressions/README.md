# Arrow function expressions

## 箭头函数的基本写法

```js
const arrowSum = (a, b) => {
  return a + b
}
```

函数表达式的写法:

```js
const arrafunctionExpressionSum = function(a, b) {
  return a + b
}
```

### 任何可以使用函数表达式的地方, 都可以使用箭头函数(*注意函数表达式与函数声明的不同*)

### 如果只有一个参数, 可以不用括号。没有参数，或者多个参数的情况下，必须使用括号

```js
const double = x => { return 2 * x }
```

### 当函数体只有一行代码, 那么可以省略大括号

```js
const triple = x => 3 * x
```

### 当返回一个对象的时候, 必须使用括号包裹

```js
const objectFunc = () => ({ a: 1 })
```

### 函数名就是指向函数的指针, 一个函数可以有多个名称

### Differences & Limitations:

- Does not have its own bindings to `this` or `super`, and should not be used as `methods`.
- Does not have `arguments`, or `new.target` keywords.
- Not suitable for `call`, `apply` and `bind` methods, which generally rely on establishing a `scope`.
- Can not be used as `constructors`.
- Can not use `yield`, within its body.

### Example

```javascript
const evens = [2, 4, 6, 8, 10]
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```