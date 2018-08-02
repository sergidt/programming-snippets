Filtering and summary exercise
=======

A partir de les següents dades: 
-----------------

```
class Person {
  constructor(public firstName: string, public lastName: string, public address: Address, public birthYear: number) {
  }

  toString() {
    return `Person(${this.firstName}, ${this.lastName})`;
  }
}

class Address {
  constructor(public country: string, public zipCode: string){}
}

class Student extends Person {
  constructor(public firstName: string, public lastName: string, public address: Address, public birthYear: number, public university: string) {
    super(firstName, lastName, address, birthYear);
  }
}

const people: Array<Person> = [
 new Student('Sergi', 'Dote', new Address('Spain', '08339'), 1979, 'UdG'),
new Person('Conor', 'Dellunay', new Address('Manchester', '322321'), 1980),
new Student('Alberto', 'Albericio', new Address('Spain', '08002'), 1975, 'UAB'),
new Person('Sophia', 'Curt', new Address('Manchester', '322321'), 1988),
new Student('Alonzo', 'Church', new Address('Dallas', '080324'), 1990, 'Princeton'),
new Student('Stephen', 'Kleene', new Address('South Africa', '1123234'), 1997, 'SAU'),
new Student('Alex', 'Albericio', new Address('Spain', '08002'), 1977, 'UAB'),
new Person('Michael', 'Saint', new Address('London', '3236382'), 2000),
new Student('Mariah', 'Clark', new Address('Pennsylvania', '232355'), 1990, 'Pennsylvania'),
new Student('John', 'McDoe', new Address('Massachussets', 'z2123213'), 1999, 'MIT'),
new Student('Eliza', 'McCan', new Address('Massachussets', 'z2123213'), 1981, 'MIT'),
new Student('Richard', 'Brandson', new Address('Massachussets', 'z2123213'), 1961, 'MIT'),
new Student('Monica', 'Sally', new Address('London', '3236382'), 2003, 'London University'),
new Student('Mark', 'Sutherlan', new Address('Los Angeles', '36382'), 2002, 'LA University')
];
```

Fer el següents exercicis: 
------

* Filtrar els estudiants:
------

```
const filterStudents = (people: Array<Person>) => people.filter(p => p instanceof Student);
```

* Filtar els estudiants del MIT:
------

```
//creem una interface que ens ajudi: 
interface Selector extends Function {
  (key: string, value: any) : (p: Person) => boolean;
}

const selector: Selector = (key: string, value: any) => (p: Person) => p[key] === value;

const MITFilter =  (people: Array<Person>) => filterStudents(people).filter(selector('university', 'MIT'));

//Podem fer coses interessants com: 
console.log(people.filter(selector('firstName', 'Sergi')));
```

* Filtrar estudiants per un país en concret: 
------
```
const findStudentsByCountry = (people: Array<Person>, country: string) => filterStudents(people).filter(p => p.address.country === country);
```

* Comptar els estudiants per país:
------
```
//ens fem un predicat d'ajuda: 
const containsCountry = (acc: any) => (country: string) => acc.hasOwnProperty(country);

//funció d'acumulació: 
const countStudentsByCountry = (people: Array<Person>) => filterStudents(people).reduce((acc, cur: Student) => {
  if (!containsCountry(acc)(cur.address.country)) {
    acc[cur.address.country] = 1;
  } else {
    acc[cur.address.country]++;
  }
  return acc;
}, {});

```



