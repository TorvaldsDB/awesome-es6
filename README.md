# Awesome-ES6 [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) by Torvalds

## Methods of merging arrays

- spread syntax

```js
let arr1 = [1, 2, 3]
let arr2 = [3, 4, 5]

let new_array = [...arr1, ...arr2]

console.log(new_array)
```

- concat

```js
let arr1 = [1, 2, 3]
let arr2 = [3, 4, 5]

let new_array = arr1.concat(arr2)

console.log(new_array)
```

- Array.prototype.unshift()

*we have to use the `apply()`*

```js
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];

//  Prepend all items from arr2 onto arr1
Array.prototype.unshift.apply(arr1, arr2)
console.log(arr1)

//  arr1 is now [3, 4, 5, 0, 1, 2]
```