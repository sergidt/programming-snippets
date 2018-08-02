# Baiscs



```

//Variable types 

//Type Assertions

//String interpolation

//Flat arrows

//Types 
boolean, number, string, Array, Tuple, Enum, any, void, null, undefined, never
//Advanced Types
Map

//Custom types

//High order functions 
map, filter, sort, some, every, find, reduce

//Interface

//Class

//Enum

//Decorator
Decorator is just a function that takes three parameters:

The target: Either the constructor function of the class for a static member, or the prototype of the class for an instance member.
The key: The name of the member, this is  the name of the method being decorated
The descriptor: The Property Descriptor for the member, this is a property descriptor of the given property if it exists on the object, undefined otherwise.
                Descriptor contains "value" property which is actual method to be invoked

export function timeout( milliseconds: number = 0 ) {
  
  return ( target, key, descriptor ) => {
    var originalMethod = descriptor.value;
    descriptor.value = function (...args) {

      setTimeout(() => {
        originalMethod.apply(this, args);
       }, milliseconds);
    };
    return descriptor;
  }
}

//////////////////////



export type AlphaNumeric = string | number;

export enum DeviceType {
    CAMERA,
    DMS,
    TSS
}


```