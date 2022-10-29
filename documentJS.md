# ES6 Document Compare With ES5

## Types
1. When you access a primitive type you work directly on its value.
<br/> Khi bạn truy cập một kiểu nguyên thủy, bạn làm việc trực tiếp trên giá trị của nó.
- string
- number
- boolean
- null
- undefined
- symbol
- bigint
```js
const foo = 1;
let bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```
2. When you access a `Data type` you work on a reference to its value
 <br/> Khi bạn truy cập một kiểu phức tạp, bạn làm việc trên một tham chiếu đến giá trị của nó
- object
- array
- funtion

```js
const foo = [1, 2];
const bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```

## References
1. If you must reassign references, use let instead of var
- Nếu bạn phải gán lại các tham chiếu, hãy sử dụng **let** thay vì **var**
```js
// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```

2. Note that both let and const are block-scoped, whereas var is function-scoped.

```js
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
  var c = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
console.log(c); // Prints 1
```


3. Use object destructuring when accessing and using multiple properties of an object.

```js
// bad
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;
}

// good
function getFullName(user) {
  const { firstName, lastName } = user;
  return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}

```

## Objects
1. Prefer the object spread syntax over Object.assign to shallow-copy objects. Use the object rest parameter syntax to get a new object with certain properties omitted.

```js
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```
## Arrays

1. Use array spreads ... to copy arrays.
```js
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```
2. To convert an iterable object to an array, use spreads ... instead of Array.from
```js
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```

## Strings
1. Use single quotes '' for strings. eslint
```js
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```

## Functions

1. immediately-invoked function expression (IIFE)
```js
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```
