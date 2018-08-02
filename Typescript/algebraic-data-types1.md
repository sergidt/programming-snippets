```
export type Alphanumeric = string | number;

export function print(value: Alphanumeric) {
    console.log(value);
}

export type Empty = void;

export type Tree = Node | Leaf;

export class Leaf {
    constructor(public value: Alphanumeric | Empty){}

    toString(): string {
        return 'Leaf: ' + this.value;
    }
}
export class Node {
    constructor(public leftChild: Tree, public rightChild: Tree){}

    toString(): string{
        return 'Node';
    }
}

const leaf: Leaf = new Leaf("I'm a leaf");
const tree: Tree =  new Node(leaf, leaf);
console.log(tree.rightChild.toString());

```