---
layout: post
title:      "Re-Iterating"
date:       2020-03-02 16:34:52 +0000
permalink:  re-iterating
---


Last week, when I was working on a project with a friend, we came to a spot where we needed to iterate over an array of objects. I immediately reached for .map() because that seems to be what I always do, but my friend stopped me and asked why I was not using .filter() in this scenario. The honest truth: I forgot .filter() even existed. 

After that I decided I needed to revisit .forEach(), .map(), and .filter() to refresh my brain on the differences and where it is appropriate to use each one.

## .forEach()

.forEach() accepts a callback and calls that callback on each element in the array in ascending order. .forEach() is not chainable, always returns 'undefined', and does not mutate the array itself. There is no way to break out of a .forEach() loop other than throwing an exception. Common uses are typically to execute side effects or sum an array:

```
const arr = [2, 4, 6, 8];
let sum = 0;

arr.forEach(element => console.log(element))

// Expected output:
// 2
// 4
// 6
// 8

arr.forEach(element => {
  sum += element
})
console.log(`Sum = ${sum}`);

// Expected output:
// Sum = 20
```
## .map()

.map() accepts a callback and calls that callback on each element in the array in ascending order, just like .forEach(). The difference here though is that .map() returns a new array with the results of each iteration. .map() does not execute the callback for elements without values, and it does not mutate the original array. A common use for .map() is when you want to perform an operation on the elements of an array and save the results:

```
const nums = [1, 2, 3, 4];

const squares = nums.map(element => element**2)
console.log(squares);

// Expected output:
// [1, 4, 9, 16]

```
An example with strings:
```
const names = [
  {first: "Jerry", last: "Seinfeld"},
  {first: "George", last: "Costanza"},
  {first: "Cosmo", last: "Kramer"}
];

const fullNames = names.map(name => {
  return name.first + " " + name.last
})
console.log(fullNames);

// Expected output:
// ["Jerry Seinfeld", "George Constanza", "Cosmo Kramer"]
```

## .filter()

.filter() accepts a callback and calls that callback on each element of the array in ascending order, just like .forEach() and .map(). The difference here is that we view the callback as a 'test'. .filter() will only return the elements that pass the test and thus *filter* out any elements that fail the test. .filter() does not mutate the original array. A common use for .filter() is to use it to eliminate any elements of an array that you don't need:

```
const nums = [2, 5, 6, 8, 11];

const evens = nums.filter(el => el%2 === 0)
console.log(evens);

// Expected output:
// [2, 6, 8]
```
An example with strings:
```
const people = ["Scooby-Doo", "Fred", "Daphne", "Shaggy", "Velma"];
const words = ["Scooby-Doo", "Villain", "Shaggy", "Fred", "Mystery Machine"];

const characters = words.filter(el => people.includes(el));
console.log(characters);

// Expected output:
// ["Scooby-Doo", "Shaggy", "Fred"]
```

## Conclusion
These three tools for iteration are really powerful and can do a lot more than what is demonstrated here, but it is important to remember that each one has its own use case. Documentation on each one can be found here:

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
