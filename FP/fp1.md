Pure Functions
--------------

```
const multiply = (a,b) => a * b;
```

**as a normal function**
``` 
function multiply(a,b) {
 return a * b;
}


console.log("pure function multiply");
console.log(multiply(10, 20));
```

Sorting example 
--------------


```
const numericSort = (n: number[]): number[] => n.sort((x, y) => x - y);

```



Currying and Partial Function Application
--------------

```
const curriedMultiply = (x: number) => (y: number) => x * y;

console.log("curried function curriedMultiply");
console.log(curriedMultiply(100)(20));
```

Generic curried function
--------------

```
function add<T>(x: T) {
 return (y: T) => x + y;
}

console.log(add(123)(123)); //-> output: 246
console.log(add("123")(123)); //-> output: "123123"

const updateStateAndVisibility = obj =>
   state =>
     visibility => Object.assign({}, obj, state, visibility);

const value = updateStateAndVisibility({type: 'camera'})({state:'online'})({visible: true});
console.log(value);

```

Curring Exercise
-----------

```
interface Node {
  label: string;
  children?: Node[];
}

  
const getChildren = (x) => x.children;

const alltheChildren = (items: Array<any>) => items.map(getChildren);
  
const flatten = (...arrays) => arrays.reduce((acc, cur) => acc.concat(...cur), []);
  
const nodes: Node[] = [{label: 'item1', children: [{label: 'item1-1'},
                                                   {label: 'item1-2'},
                                                   {label: 'item1-3'}]},
                       {label: 'item2', children: [{label: 'item2-1'},
                                                   {label: 'item2-2'},
                                                   {label: 'item2-3'}]}];

  const children = alltheChildren(nodes);
console.log(flatten(children));
```

Simple Function composition
--------------
```
const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);
const newFunc = pipe(fn1, fn2, fn3);
const result = newFunc(arg);


//  COMPOSITION:
  
const compose = (f, g) => (x)  => f(g(x));
  
  const toUpperCase = (x) => x.toUpperCase();

const exclaim = (x) => x + '!';

const shout = compose(exclaim, toUpperCase);

console.log(shout("send in the clowns"));
  


//Example 2
  
const head = (x) => x[0];

const reverse = (list) => list.reduce((acc, x) => [x].concat(acc), []);
const last = compose(head, reverse);
  

console.log(last(['jumpkick', 'roundhouse', 'uppercut']));
  
Other exercise:
const add = (a, b) => a + b;

const average = (nums: number[]): number => nums.reduce(add) / nums.length;




//Example 3

const getlength = (param: string | Array<any>) => param.length;

const split = (sentence: string) => sentence.split(' ');

const wordCount = compose(getlength, split);

console.log(wordCount('hello i am sergi'));


```



