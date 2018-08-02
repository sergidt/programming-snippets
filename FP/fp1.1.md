compose functions
--------------
```

function compose(args, funct) {
 return args.reduce((a,b) => funct(a)(b));
}

const add = (a) => (b) => a + b;
```

we can execute that
-----------
```
console.log(compose([1, 2, 3], add));  
console.log(compose(['a', 'b', 'c'], add)); 
 
function multiply(a,b) {
 return a * b;
}

console.log("pure function multiply");
console.log(multiply(10, 20));
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

Simple Function composition
--------------
```
const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);
const newFunc = pipe(fn1, fn2, fn3);
const result = newFunc(arg);


const square = (x) => x * x;
const double = (x) => x * 2;

const newFunc = pipe(square, double);
const result = console.log(newFunc(4));
```