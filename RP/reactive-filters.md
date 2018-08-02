```
import { Component, OnInit } from '@angular/core';
import { BehaviorSubject } from 'rxjs/BehaviorSubject';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/filter';
import 'rxjs/add/operator/combineLatest';
import 'rxjs/add/operator/do';
import 'rxjs/add/operator/startWith';

import { Subject } from 'rxjs/Subject';

type Type = string;

interface Subtype {
    type: string;
    subTypes: string[];
}

interface Device {
    name: string;
    type: string;
    subType: string;
}

@Component({
    selector: 'filters',
    template: `
                <form>
                  <div><label id="deviceType">Device:</label>
                    <select (change)="typeSelected($event.target.value)">
                      <option *ngFor="let type of types">{{ type }}
                      </option>
                    </select>
                  </div>
                  <br>
                  <div>
                    <label id="deviceSubtype">Sub type:</label>
                    <select (change)="selectedSubType$.next($event.target.value)">
                      <option *ngFor="let subType of subTypes$ | async">{{ subType }}
                      </option>
                    </select>
                  </div>
                </form>
                <div *ngFor="let device of devices$ | async">{{device.type}} {{device.subType}}: {{ device.name }}</div>`,
    styles: [`
        #deviceType {
            padding-right: 30px;
        }

        #deviceSubtype {
            padding-right: 16px;
        }
    `]
})
export class FiltersComponent implements OnInit {
    types: Type[] = [ 'Camera' ,'DMS' ];

    subTypes: Subtype[] = [
        { type: 'Camera', subTypes: ['Movable', 'Fixed'] },
        { type: 'DMS', subTypes: ['1B6S', 'GT', '1B'] }
    ];

    devices: Device[] = [
        { name: 'Diagonal', type: 'DMS', subType: '1B6S' },
        { name: 'Diagonal2', type: 'DMS', subType: '1B6S' },
        { name: 'Meridiana', type: 'DMS', subType: '1B' },
        { name: 'Meridiana2', type: 'DMS', subType: '1B' },
        { name: 'Rambla Catalunya', type: 'DMS', subType: 'GT' },
        { name: 'Rambla Catalunya2', type: 'DMS', subType: 'GT' },
        { name: 'Rambla Catalunya', type: 'Camera', subType: 'Movable' },
        { name: 'Plaça Catalunya', type: 'Camera', subType: 'Movable' },
        { name: 'Gran via km1', type: 'Camera', subType: 'Fixed' },
        { name: 'Passeig de Gràcia', type: 'Camera', subType: 'Fixed' }
    ];

    selectedType$: BehaviorSubject<Type> = new BehaviorSubject(this.types[0]);
    subTypes$: BehaviorSubject<string[]> = new BehaviorSubject(null);
    selectedSubType$: BehaviorSubject<string> = new BehaviorSubject(null);
    devices$: Subject<Device[]> = new Subject();

    ngOnInit() {
      this.selectedType$
          .map(type => this.subTypes.find(subType => subType.type === type))
          .map(subtype => ['', ...subtype.subTypes])
          .subscribe(this.subTypes$);

      this.selectedType$
          .combineLatest(this.selectedSubType$)
          .map(([type, subType]) => this.filterDevices(type, subType))
          .subscribe(this.devices$);
    }

    typeSelected(type) {
        this.selectedType$.next(type);
        this.selectedSubType$.next(null);
    }

    filterDevices = (type, subType) => this.devices.filter(device => device.type === type && device.subType === subType);
}
```
