# JS-Cheat-Sheet
Hopefully best practices in modern JS.

## Variable declarations
### var, let & const
`const` is the most strict declaration and can't be reassigned. It should be used as default whereever possible.

`let` is the second strict declaration and can be reassigned. It should be used for iteration counter.

`var` is the less strict declaration and should be avoided when possible.


Once variables with `const` or `let` are declared, any attemp to declare them again will fail.

### Shorthands
```
const [t, u] = ['a', 'b'];
t; // 'a'
u; // 'b'
```

```
const { type, payload } = action;
type; // content of action.type
payload; // content of action.payload
```

## Types
### Arrays
An ordered list of values.

`[1, 2, 3];` -> The literal notation.

`const arr = [1, 2, 3];` -> With a given name.

### Objects
An object in JavaScript is a collection of key: value pairs.

`{ key: 'value' }` -> The literal notation.

`const foo = { bar: 'bar' }` -> With a given name.

#### Composing Objects
Objects can be easily composed together into new objects with `Object.assign()`.

```
const oA = { a: 'a' };
const oB = { b: 'b' };
const c = Object.assign({}, oA, oB); // c becomes { a: 'a', b: 'b' }
```

Note that when you use `Object.assign()`, you must pass a destination object as the first parameter. It is the object that properties will be copied to. If you forget, and omit the destination object, the object you pass in the first argument will be mutated.

## Comparisons
Use the `===` comparison whenever possible. It has strict type cheching:

```
3 + 1 === 4; // true
3 + 1 === '4'; // false
```

Using just `==` has no type checking:

```
3 + 1 == 4; // true
3 + 1 == '4'; // true
```

## Ternaries (IF shorthand)
`14 - 7 === 7 ? 'Yep!' : 'Nope.'; // Yep!`

## Functions
```
const double = function(x) {
   return x * 2;
}
```

equals

`const double = x => x * 2;`

### Default parameter values
```
const orZero = (n = 0) => n;
orZero(); // 0
orZero(2); // 2
orZero(undefined); // 0
```

### Named arguments
```
const createUser = ({
   name = 'Anonymous',
   avatarThumbnail = '/anonymous.png'
   }) => ({
     name,
     avatarThumbnail
   });

const george = createUser({
   name: 'George',
   avatarThumbnail: 'george.png'
});
george;
/*
{
   name: 'George',
   avatarThumbnail: 'george.png'
}
*/
```

### Rest and Spread
For example, the following function simply discards the first argument and returns the rest as an array:

```
const aTail = (head, ...tail) => tail;
aTail(1, 2, 3); // [2, 3]
```

Spread does the opposite: it spreads the elements from an array to individual elements. Consider this:

```
const shiftToLast = (head, ...tail) => [...tail, head];
shiftToLast(1, 2, 3); // [2, 3, 1]
```
