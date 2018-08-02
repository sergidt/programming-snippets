Counting predicates
===

**Exercise**: How to find the first item after the second number in an array.
It is simple to find every number in the given Array.

```
const isNumber => typeof x === 'number';
const list = ['a', 'b', 1, 2, 3, 'c'];
console.log(list.filter(isNumber)); // [1, 2, 3]

````

The .filter iterator returns every value that passes the given predicate. What if we wanted to know what the second number is in an array? We could filter and access element with index 1.

```
const nth = (k, predicate) => (list) => list.filter(predicate)[k];

const secondNumber = nth(1, isNumber);
console.log(secondNumber(list)); // 2
```

This approach destroys the information about the position of the second number in the original list. What if I wanted to print first item after the second number? **By filtering the list first, we lost the original positions.**

## Counting predicates ##

Instead of making a new array right away, we can write a function that keeps count how many times a predicate returned true:

```
console.clear();

const isNumber = (x) => typeof x === 'number';

const list = ['a', 'b', 1, 2, 3, 'c'];

const nth = (n, predicate) => (list) => {
    let i = 0;
    return  list.map(x => (i++) - 1 === n && predicate(list[n]));
};

console.log(nth(2, isNumber)(list));
```

or:
```
const nth = (n, predicate) => (list) => predicate.apply(null,list);
```