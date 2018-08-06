TypeScript is a typed superset of JavaScript. I would like to share with you some of my preferred tips ans tricks that have made me love this language.


## Accessing deep properties safely

Accessing deep properties of an Object in a safe way is not easy stuff, we all know that. We have all faced accessing a property, at some level, with a null value. 

Using TypeScript, we can use the reduce function to safely access a property and ensure that its full access chain is available. 

Supose the following data model:

```
interface Person {
    fullName: string;
    address: Address;
    age: number
}

interface Address {
    city: City;
    street: string;
}

interface City {
    name: string;
    postalCode: string;
}

let person: Person = {
    fullName: 'Sergi Dote',
    age: 36,
    address: {
        street: 'SouthStreet, 13',
        city: {
            name: 'Barcelona',
            postalCode: '08080'
        }
    }
};
```

Now, if we want to access some property's value, we can write a new function that handles this for us:

```
function getPropertyValue(obj: any, keys: string): any {
    return keys.split('.').reduce((value, currentKey) => !!value ? value[currentKey] : value, obj);
}
```

Using this function, we can safely navigate through the Object and access the value of whatever property we want, at whatever nested level: 

```
    getPropertyValue(person, 'address.city.name');
```

Accessing a property's value is as easy as defining a dotted string notation as a second parameter to our function.

The function will only return null or the desired result. Using this approach we are safely bypassing possible Runtime errors that may occur during a regular inspection of an Object.


## Updating multiple properties of an object at the same time

Updating multiple fields of an Object in a single shot is also a very common use case in modern applications.

To accomplish this, let's write a second function using the TypeScript `keyof` type operator. 

Using `keyof` operator we're effectively limiting parameters to be only accepted properties and types for our Object.

Finally, combining `keyof` with generics, we can create an improved and more powerful version of our new function: 

```
function updateProperties<T, K extends keyof T>(obj: T, props: Array<[K, any]>) {
    let newObject: T = Object.assign({}, obj);

    props.forEach(([k, v]: [K, any]) => newObject[k] = v);
    return newObject;
}
```

Let's see our new function in action:

```
updateProperties(person, [['fullName', 'Sergi Dote Teixidor'], ['age', 37]]);
```

We can make of use tuples to indicate what properties we want to update. 

In the example above, we are updating fullname and age. Since this is a pure function, we're not updating and returning the same instance of the Object but a new fresh Person. This technique will effectively match Inmutability patterns.


## Support for mix-in classes 
TypeScript 2.2 added support for the ECMAScript 2015 mixin class pattern.

Extracted from the TypeScript documentation, we can say that: 

- A mixin constructor type refers to a type that has a single construct signature with a single rest argument of type any[] and an object-like return type. For example, given an object-like type X, new (...args: any[]) => X is a mixin constructor type with an instance type X.

- A mixin class is a class declaration or expression that extends an expression of a type parameter type. The following rules apply to mixin class declarations:

- The type parameter type of the extends expression must be constrained to a mixin constructor type.

- The constructor of a mixin class (if any) must have a single rest parameter of type any[] and must use the spread operator to pass those parameters as arguments in a super(...args) call.

- Given an expression Base of a parametric type T with a constraint X, a mixin class class C extends Base {...} is processed as if Base had type X and the resulting type is the intersection typeof C & T. 


Using these assertions, you can simulate multiple inherintance.

Supose the following model: 

```
class Base {
    public getName(): string {
        return 'i am a base class';
    }
}

const Storage = Base => class extends Base {
    save(json: JSON) {
        console.log('JSON saved');
    }
}

const Validation = Base => class extends Base {
    validate() {
        console.log('validated');
    }
}

class Process extends Storage(Validation(Base)) {
    constructor() {
        console.log('i am a process');
    }
}
```

We can create a Process that effectively inherits the behaviour of multiple classes:

```
let process: Process = new Process();

process.validate();
process.save();
console.log(process.getName());
```

Getting this output: 

```

i am a process
(unknown) validated
(unknown) JSON saved
(unknown) i am a base class
```


## Conclusion

We have just gone through some basic yet powerful TypeScript features. Imagine how you can enrich, and empower, your code just by using out-of-the box operators, functional programming, generics and other great stuff.

Stay tuned for new posts where we will cover more operators, technologies and patterns that solve real world scenarios.
