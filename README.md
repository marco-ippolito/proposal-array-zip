# Array.prototype.zip for JavaScript

Champions: Michele Riva

Author: Marco Ippolito

Stage: 0

This is a proposal to add the method `zip` to JavaScript's built-in `Array` class.

## Motivation

Addressing element-wise array combination in JavaScript is a common problem.
The existing solutions in JavaScript lack ergonomic and performance.
The absence of a dedicated `Array.prototype.zip` method leaves developers resorting to intricate workarounds.
These workarounds are not efficient and potentially potentially error prone.

## Comparison with other languages

See [other languages document](./other-languages.md) to get overview of `Array.prototype.zip` methods in other languages.

## Proposal

The `Array.prototype.zip` method enriches the JavaScript Array class with a tool for element-wise array combination.

This would add the following method:

* `Array.prototype.zip(...arrays)`
* `Array.prototype.zip(...arrays, mapFn)`
* `Array.prototype.zip(...arrays, mapFn, thisArg)`

### Parameters

* `args` - Arrays to zip with the original array.
* `mapFn` - Function to call on every element of the zipped arrays. It accepts the following arguments:
  * `...args` - N-th elements of the zipped arrays in order.
  * `index` - The index of the element in the original array.
  * `array` - The original array.
* `thisArg` - Value to use as `this` when executing `mapFn`.

## Examples

```js
// zips two arrays
['a', 'b', 'c'].zip([1, 2, 3]) // [['a', 1], ['b', 2], ['c', 3]]

// zips three arrays
['a', 'b', 'c'].zip([1, 2, 3], [true, false, true]) // [['a', 1, true], ['b', 2, false], ['c', 3, true]]
```

If arrays have different sized, the resulting array will have the length of the shortest array:

```js
['a', 'b', 'c'].zip([1, 2]) // [['a', 1], ['b', 2]]
```

Using the mapFn argument:

```js
['a', 'b', 'c'].zip([1, 2, 3], (a, b, index, array) => ({ [a]: b })) // [{ a: 1}, {b: 2}, {c: 3}]
```

Using the mapFn with 3 arguments:

```js
['a', 'b', 'c'].zip([1, 2, 3], [true, false, true], (a, b, c, index, array) => ({ [a]: c ? b : null })) // [{ a: 1}, {b: null}, {c: 3}]
```

Using the mapFn and thisArg arguments:

```js
['a', 'b', 'c'].zip([1, 2, 3], (a, b, index, array)  =>  ({ [a]: b + this.offset }), { offset: 10 }) // [{ a: 11}, {b: 12}, {c: 13}]
```
