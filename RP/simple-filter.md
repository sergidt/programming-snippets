Needed interfaces
----
```
interface Device {
    name: string;
    active: boolean;
}

interface Filter {
    field: string;
    value: any;
}

const printDevice = (d: Device) => `name: ${d.name}, is active? ${d.active}`;
```
Filter operation
----
```
interface FilterOperation<T extends Device> extends Function {
    (devices: T[], filter: Filter): T[];
}

const applyFilter: FilterOperation = (devices: Device[], filter: Filter) => devices.filter(device => device[filter.field] === filter.value);
```
Data
----
const devices = Observable.of([
    <Device>{name: 'Device 1', active: true},
    <Device>{name: 'Device 2', active: false},
    <Device>{name: 'Device 3', active: true},
    <Device>{name: 'Device 4', active: false},
]);

Use
----
```
const filter = {field: 'active', value: true};

const filters = new BehaviorSubject(filter);

Observable.combineLatest(devices, filters)
          .map(([devices, filter]) => applyFilter(devices, filter))
          .subscribe(devices => devices.forEach(device => console.log(printDevice(device))));
```



