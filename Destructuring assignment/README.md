# Destructuring assignment

> The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

> Destructuring allows binding using pattern matching, with support for matching arrays and objects. Destructuring is fail-soft, similar to standard object lookup foo["bar"], producing undefined values when not found.

## Assignment without declaration

```js
let a, b;

({a, b} = {a: 1, b: 2});
```

> **Note**: The parentheses ( ... ) around the assignment statement are required when using object literal destructuring assignment without a declaration.
>
> {a, b} = {a: 1, b: 2} is not valid stand-alone syntax, as the {a, b} on the left-hand side is considered a block and not an object literal.
>
> However, ({a, b} = {a: 1, b: 2}) is valid, as is const {a, b} = {a: 1, b: 2}
>
> Your ( ... ) expression needs to be preceded by a semicolon or it may be used to execute a function on the previous line.

## Unpacking fields from objects passed as a function parameter

```js
const user = {
  id: 42,
  displayName: 'jdoe',
  fullName: {
    firstName: 'John',
    lastName: 'Doe'
  }
};

function userId({id}) {
  return id;
}

function whois({displayName, fullName: {firstName: name}}) {
  return `${displayName} is ${name}`;
}

console.log(userId(user)); // 42
console.log(whois(user));  // "jdoe is John"
```

## Setting a function parameter's default value

```js
function drawChart({size = 'big', coords = {x: 0, y: 0}, radius = 25} = {size: 'small'}) {
  console.log(size, coords, radius);
  // do some chart drawing
}

drawChart({
  coords: {x: 18, y: 30},
  radius: 30
});

drawChart();
```

### For of iteration and destructuring

```js
const people = [
  {
    name: 'Mike Smith',
    family: {
      mother: 'Jane Smith',
      father: 'Harry Smith',
      sister: 'Samantha Smith'
    },
    age: 35
  },
  {
    name: 'Tom Jones',
    family: {
      mother: 'Norah Jones',
      father: 'Richard Jones',
      brother: 'Howard Jones'
    },
    age: 25
  }
];

for (const {name: n, family: {father: f, sister: s = 'no name'}, nobody: {hello: h} = {hello: 'who'}} of people) {
  console.log(`Name: ${n}, Father: ${f}, Sister: ${s}, Hello ${h}`);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```

## The prototype chain is looked up when the object is deconstructed

When deconstructing an object, if a property is not accessed in itself, it will continue to look up along the prototype chain.

```js
let obj = {self: '123'};
obj.__proto__.prot = '456';
const {self, prot} = obj;
// self "123"
// prot "456" (Access to the prototype chain)
```