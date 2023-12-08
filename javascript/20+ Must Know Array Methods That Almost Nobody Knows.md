# 20+ Must Know Array Methods That Almost Nobody Knows

[link on youtube](https://www.youtube.com/watch?v=mSBnJvHtgD0)

# `Object.gronpBy()`

The `Object.groupBy()` static method *groups* the elements of a given iterable according to the string values returned by a provided callback function.

> Note : limited availability, check your browser first

> Note : Not available in Node.js, to use it, take a look into [core-js](https://github.com/zloirock/core-js)

```js
import "core-js/stable/object/index.js";

const people = [
  { name: "Wes", age: 28 },
  { name: "Chad", age: 45 },
  { name: "Bob", age: 37 },
  { name: "Doug", age: 21 },
  { name: "Chad", age: 18 },
];

const a = Object.groupBy(people, (person) => person.name);

console.log(a);
```

result:

```js
[Object: null prototype] {
  Wes: [ { name: 'Wes', age: 28 } ],
  Chad: [ { name: 'Chad', age: 45 }, { name: 'Chad', age: 18 } ],
  Bob: [ { name: 'Bob', age: 37 } ],
  Doug: [ { name: 'Doug', age: 21 } ]
}
```

You can also group by (for example) the first character of the name:

```js
import "core-js/stable/object/index.js";

const people = [
  { name: "Wes", age: 28 },
  { name: "Carter", age: 45 },
  { name: "Bob", age: 37 },
  { name: "Doug", age: 21 },
  { name: "Chad", age: 18 },
];

const a = Object.groupBy(people, (person) => person.name[0]);

console.log(a);
```

result:

```js
[Object: null prototype] {
  W: [ { name: 'Wes', age: 28 } ],
  C: [ { name: 'Carter', age: 45 }, { name: 'Chad', age: 18 } ],
  B: [ { name: 'Bob', age: 37 } ],
  D: [ { name: 'Doug', age: 21 } ]
}
```

# `shift()` and `unshift()`

`shift()` removes the *first* element from an array and returns that removed element. This method changes the length of the array.

These are the opposed methods of `pop()` and `push()` which add and remove elements from the *end* of an array.

```js
const names = ["Wes", "Kait", "Lux"];

const a = names.shift();

console.log(a); // Wes
console.log(names); // [ 'Kait', 'Lux' ]
```

`unshift()` adds one or more elements to the *beginning* of an array and returns the new length of the array.

```js
const names = ["Wes", "Kait", "Lux"];

const a = names.unshift("Bob");

console.log(a); // 4
console.log(names); // [ 'Bob', 'Wes', 'Kait', 'Lux' ]
```

# `with()`

The `with()` function is the *copying* version of usin the bracket notation to access an array element, it returns a new array with the element at the specified index replaced by the given value.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.with(1, "Bob");

console.log(a); // [ 'Wes', 'Bob', 'Lux' ]
console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

You can use negative numbers to access elements from the end of the array:

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.with(-1, "Bob");

console.log(a); // [ 'Wes', 'Kait', 'Bob' ]
console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

# `at()`

The `at()` method returns a new array with the specified elements from the original array using the index, it also allows the negative index.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.at(1);

console.log(a); // [ 'Kait' ]
console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

You can also use negative numbers to access elements from the end of the array:

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.at(-1);

console.log(a); // [ 'Lux' ]
console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

# `fill()`

The `fill()` method fills (modifies) all the elements of an array from a start index (default zero) to an end index exclusively (default array length) with a static value. It returns the modified array.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.fill("Bob");

console.log(a); // [ 'Bob', 'Bob', 'Bob' ]
console.log(names); // [ 'Bob', 'Bob', 'Bob' ]
```

You can also specify the start and end index:

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.fill("Bob", 1, 2);

console.log(a); // [ 'Wes', 'Bob', 'Lux' ]
console.log(names); // [ 'Wes', 'Bob', 'Lux' ]
```

# `toReversed()`

The `toReversed()` method reverses an array. Unlike the `reverse()` method which mutates the original array, `toReversed()` returns a new array with the elements reversed and keeps the original array intact.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.toReversed();

console.log(a); // [ 'Lux', 'Kait', 'Wes' ]
console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

# `toSorted()`

The `toSorted()` method sorts the elements of an array in place and returns the sorted array. Unlike the `sort()` method which mutates the original array, `toSorted()` returns a new array with the elements sorted and keeps the original array intact.

The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.toSorted();

console.log(a); // [ 'Kait', 'Lux', 'Wes' ]

console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

You can also specify a compare function:

```js

import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.toSorted((a, b) => a.length - b.length);

console.log(a); // [ 'Lux', 'Wes', 'Kait' ]

console.log(names); // [ 'Wes', 'Kait', 'Lux' ]
```

# `toSpliced()`

The `toSpliced()` method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place. Unlike the `splice()` method which mutates the original array, `toSpliced()` returns a new array with the elements spliced and keeps the original array intact.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", "Kait", "Lux"];

const a = names.toSpliced(1, 1, "Bob");

console.log(a); // [ 'Kait' ]

console.log(names); // [ 'Wes', 'Bob', 'Lux' ]
```

# `flat()`

The `flat()` method creates a new array with sub-array elements concatenated into it recursively up to the specified depth.

The default depth is 1. You can specify the depth as an argument, or Infinity to flatten the nested array completely.

```js
import "core-js/stable/array/index.js";

const names = ["Wes", ["Kait", ["Lux"]]];

const a = names.flat();

console.log(a); // [ 'Wes', 'Kait', [ 'Lux' ] ]

console.log(names); // [ 'Wes', [ 'Kait', [ 'Lux' ] ] ]
```

You can also specify the depth:

```js

import "core-js/stable/array/index.js";

const names = ["Wes", ["Kait", ["Lux"]]];

const a = names.flat(2);

// or

const b = names.flat(Number.POSITIVE_INFINITY);

console.log(a); // [ 'Wes', 'Kait', 'Lux' ]

console.log(b); // [ 'Wes', 'Kait', 'Lux' ]

console.log(names); // [ 'Wes', [ 'Kait', [ 'Lux' ] ] ]
```

# `findLast()`

The `findLast()` method returns the value of the last element in the provided array that satisfies the provided testing function. Otherwise undefined is returned.

```js

import "core-js/stable/array/index.js";

const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const a = numbers.findLast((number) => number % 2 === 0);

console.log(a); // 8

console.log(numbers); // [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

# `reduceRight()`

The `reduceRight()` method applies a function against an accumulator and each value of the array (from right-to-left) to reduce it to a single value.

This is the same of using `reverse()` and `reduce()`.

```js

import "core-js/stable/array/index.js";

const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const a = numbers.reduceRight((accumulator, number) => {
  return accumulator + number;
}, 0);

console.log(a); // 45

console.log(numbers); // [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

# `Array.isArray()`

The `Array.isArray()` method determines whether the passed value is an Array.

```js

import "core-js/stable/array/index.js";

const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const a = Array.isArray(numbers);

console.log(a); // true

console.log(numbers); // [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

> Note : using `typeof` on an array returns `object`, which is not very helpful.
