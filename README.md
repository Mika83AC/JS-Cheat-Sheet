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

#### Array.filter()
```
const greater2 = x => x > 2;
[2, 4, 6].filter(greater2); // [4, 6]
```

#### Array.map()
```
const double = x => x * 2;
[1, 2, 3].map(double); // [2, 4, 6]
```

In this case, `arr` is the object, `.map()` is a property of the object with a function for a value. When you invoke it, the function gets applied to the arguments, as well as a special parameter called `this`, which gets automatically set when the method is invoked. The `this` value is how `.map()` gets access to the contents of the array.

Note that the original arr value is unchanged:

`arr; // [1, 2, 3]`

#### Array.reduce()
```
const arr = [1, 2, 3];
var totalAmount = arr.reduce(function(sum, item) {
   return sum + item
}, 0)
totalAmount; // 6
```

equals

```
const arr = [1, 2, 3];
var totalAmount = arr.reduce((sum, item) => sum + item, 0)
totalAmount; // 6
```

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

IMPORTANT: `=>` lacks its own `this`, and can't be used as a constructor.

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

### Currying
```
const highpass = function highpass(cutoff) {
  return function (n) {
    return n >= cutoff;
  };
};
```

equals

`const highpass = cutoff => n => n >= cutoff;`

Since `highpass()` returns a function, you can use it to create a more specialized function:

```
const gt4 = highpass(4);
gt4(6); // true
gt4(3); // false
```

Autocurry lets you curry functions automatically, for maximal flexibility. Say you have a function `add3()`:

`const add3 = curry((a, b, c) => a + b + c);`

With autocurry, you can use it in several different ways, and it will return the right thing depending on how many arguments you pass in:

```
add3(1, 2, 3); // 6
add3(1, 2)(3); // 6
add3(1)(2, 3); // 6
add3(1)(2)(3); // 6
```

### Function composition
```
const inc = n => n + 1;
inc(double(2)); // 5
```

The value `2` is passed into `double()`, which produces `4`. `4` is passed into `inc()` which evaluates to `5`.

You can pass any expression as an argument to a function. The expression will be evaluated before the function is applied:

`inc(double(2) * double(2)); // 17`

Since `double(2)` evaluates to `4`, you can read that as `inc(4 * 4)` which evaluates to `inc(16)` which then evaluates to `17`.

### Method Chaining
```
const arr = [1, 2, 3];
arr.map(double).map(double); // [4, 8, 12]
```

### Higher-Order-Functions
How JavaScripts Array.filter() (a very very flexible base function!) is build:

```
const reduce = (reducer, initial, arr) => {
   let acc = initial;
   for (let i = 0, length = arr.length; i < length; i++) {
      acc = reducer(acc, arr[i]);
   }
   return acc;
};

const filter = (fn, arr) =>
   reduce((acc, curr) =>
      fn(curr) ? acc.concat([curr]) : acc, [], arr
);

const wordArray = ['oops', 'gasp', 'shout', 'sun'];
const censor = words => filter(word => word.length !== 4, words);
const startsWithS = words => filter(word => word.startsWith('g'), words);

console.log(censor(wordArray)); // [ 'shout', 'sun' ]
console.log(startsWithS(wordArray)); // [ 'gasp' ]
```

Source: https://medium.com/javascript-scene/higher-order-functions-composing-software-5365cf2cbe99
