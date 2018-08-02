Higher Order Functions
=======

*Closures, function factories, common factory pattern functions*

Creating Closures
------

Consider the code below
```
const add = (x) => (y) => x + y;

var add2 = add(2)
var add3 = add(3)

add2(4) //==>> 6
add3(6) //==>> 9
```

So why not just write add as add(x, y)? This is surely simpler. Writing functions as function factories allow you to compose other functions. We will see more of this below. 

Other functions that are written using the function factory pattern.

get
---
```
const get = (prop) => (obj) => obj[prop];

//As ES5 notation: 

function get(prop) {
  return function(obj) {
    return obj[prop]
  }
}
```

What is interesting about this function is that it does no “getting” so to speak. We can create other functions: 

```
const getName = get(“name”);
```

getName gets set to a function returned by get. But before returning anything, get creates a closure with variable prop (property) for getName. And the property is set to “name”.

```
const book = {
  name: “FP JavaScript”
}
getName(book) //==>> FP JavaScript

//get can be used with arrays too.

get(1)([1, 2, 3]) //==>> 2
```

This is useful for accessing the same property of a set of objects:

```
const  people = [ {name:”John”, email:”john@example.com”},
 {name:”Bill”, email:”bill@example.com”} ];

 const people = [ {name:'John', email:'john@example.com'},
 {name:'Bill', email:'bill@example.com'} ];

console.log(people.map(get('email')));  //===>> [ ‘john@example.com’, ‘bill@example.com’ ]
```

Pluck
----

The pattern *map(get())* is so common that we a have a function for it called pluck.
```
const pluck = (prop) => (array) => array.map(get(prop));

pluck(‘email’)(people);  //===>> [ ‘john@example.com’, ‘bill@example.com’ ]
```

Take
-----

take takes a number n and returns a function, to which you must pass an array, to get the first n elements of the array:
```
const take = (n) => (arr) => arr.slice(0, n);
```

