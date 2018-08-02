# Return types as inference targets

For one, TypeScript can now make inferences for the return type of a call. This can improve your experience and catch errors. Something that now works:

```
function arrayMap<T, U>(f: (x: T) => U): (a: T[]) => U[] {
    return a => a.map(f);
}

const lengths: (a: string[]) => number[] = arrayMap((s: string) => s.length);
console.log(lengths(['sergi', 'dote', 'teixidor']));


const range = (start, end) => Array.from({ length: end - start + 1 }, (x, i) => i + start);

///
const is = p => v => o => o.hasOwnProperty(p) && o[p] == v;

```