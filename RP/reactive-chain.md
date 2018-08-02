Needed interfaces
----
```
interface ConnectableFunction extends Function {
  (params?: any) : Observable<any>;
}

Observable.from([1,2,3,4])
.reduce((acc, cur) => acc + cur)
.subscribe(console.log);

function chainReactiveFunctions(functions: Array<ConnectableFunction>, flattenResult: boolean = false,
    initialValue: any = undefined): Observable<any> {
    const initial = initialValue ? (initialValue instanceof Observable ? initialValue : Observable.of(initialValue)) :
        functions[0]();

    const list = functions.slice(initialValue ? 0 : 1);

    const result = Observable.from(list).reduce((acc, cur) => acc.switchMap(_ => cur(_)), initial);

    return flattenResult ? result.switchMap(_ => _) : result;
}

const add1 = (num: number) => Observable.of(num+1);
const square = (num: number) => Observable.of(num*num);
  
  chainReactiveFunctions([add1, square])
  .subscribe(console.log)
```



