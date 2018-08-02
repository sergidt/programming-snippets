Functional programming 
==

For most of us, the real benefit of adopting a functional style is that our programs can be broken down into smaller, simpler pieces that are both more reliable and easier to understand. 

From a more pragmatic perspective, a beginner needs to understand only two concepts in order to use it for everyday applications.

Data in functional programs should be immutable
--

which sounds serious but just means that it should never change. In practice, you would simply create new data structures instead of modifying ones that already exist. 

functional programs should be stateless
--
Basically means they should perform every task as if for the first time, with no knowledge of what may or may not have happened earlier in the program’s execution. 

This means that your functions will operate only on data passed in as arguments and will never rely on outside values to perform their calculations.

**Example**

For example, let’s say we have an API response:
```
const data = [
  { 
    name: "Jamestown",
    population: 2047,
    temperatures: [-34, 67, 101, 87]
  },
  {
    name: "Awesome Town",
    population: 3568,
    temperatures: [-3, 4, 9, 12]
  }
  {
    name: "Funky Town",
    population: 1000000,
    temperatures: [75, 75, 75, 75, 75]
  }
];
```
We want to group temperatures average and population:
[
  [x, y],
  [x, y]
  …etc
]
Here, x is the average temperature, and y is the population.

Without functional programming:

``` javascript
var coords = [],
    totalTemperature = 0,
    averageTemperature = 0;

for (var i=0; i < data.length; i++) {
  totalTemperature = 0;
  
  for (var j=0; j < data[i].temperatures.length; j++) {
    totalTemperature += data[i].temperatures[j];
  }

  averageTemperature = totalTemperature / data[i].temperatures.length;

  coords.push([averageTemperature, data[i].population]);
}
```

When programming in a functional style, you’re always looking for simple, repeatable actions that can be abstracted out into a function. 

We can then build more complex features by calling these functions in sequence (composing functions). 

At a basic level, we’ll perform the following actions on our data:

* add every number in a list,
* calculate an average,
* retrieve a single property from a list of objects.

Rules to ensure that you’re following best practices:
* All of your functions must accept at least one argument.
* All of your functions must return data or another function.
* No loops!

OK, let’s add every number in a list. 

const totalForArray = (currentTotal, arr) => arr.length === 0 ? currentTotal : totalForArray(currentTotal + arr[0], arr.slice(1));


To get the average of an array:

const average = (total, count) => total / count;

const averageForArray = (arr) => average(totalForArray(arr), arr.length);
