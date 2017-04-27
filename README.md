<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [JS-Cheat-Sheet](#js-cheat-sheet)
  - [Variable declarations](#variable-declarations)
    - [var, let & const](#var-let--const)
      - [Shorthands with ES6 destructuring](#shorthands-with-es6-destructuring)
  - [Types](#types)
    - [Arrays](#arrays)
      - [Array.filter()](#arrayfilter)
      - [Array.map()](#arraymap)
      - [Array.reduce()](#arrayreduce)
    - [Objects](#objects)
      - [Composing Objects](#composing-objects)
  - [Comparisons](#comparisons)
  - [Ternaries (IF shorthand)](#ternaries-if-shorthand)
  - [Functions](#functions)
    - [Default parameter values](#default-parameter-values)
    - [Parameter destructuring with ES6](#parameter-destructuring-with-es6)
    - [Named arguments](#named-arguments)
    - [Recursion](#recursion)
    - [Rest and Spread](#rest-and-spread)
    - [Currying](#currying)
    - [Function composition](#function-composition)
      - [compose(), be careful](#compose-be-careful)
    - [Method Chaining](#method-chaining)
    - [Higher-Order-Functions](#higher-order-functions)
    - [Pure functions](#pure-functions)
  - [Monads, Functors, and Fancy Words](#monads-functors-and-fancy-words)
  - ["Classes", Factories, inheritance and it's best practices](#classes-factories-inheritance-and-its-best-practices)
    - [Factory function (use if possible!)](#factory-function-use-if-possible)
    - [ES5 constructor function (don't use if possible)](#es5-constructor-function-dont-use-if-possible)
    - [ES6 class (don't use if possible)](#es6-class-dont-use-if-possible)
    - [Inheritance](#inheritance)
    - [Concatenative Inheritance / Cloning / Mixins](#concatenative-inheritance--cloning--mixins)
    - [Functional Inheritance](#functional-inheritance)
    - [!!! Composition !!!](#-composition-)
      - [Composition with Stamps (stampit.js)](#composition-with-stamps-stampitjs)
- [Examples of nice functional programming](#examples-of-nice-functional-programming)
  - [The greeting mess](#the-greeting-mess)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# JS-Cheat-Sheet
Hopefully best practices in modern JavaScript (ES6 and beyond).

## Variable declarations
### var, let & const
`const` is the most strict declaration and can't be reassigned. It should be used as default whereever possible.

`let` is the second strict declaration and can be reassigned. It should be used for iteration counter.

`var` is the less strict declaration and should be avoided when possible.


Once variables with `const` or `let` are declared, any attemp to declare them again will fail.

#### Shorthands with ES6 destructuring
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
const summingReducer = (acc, n) => acc + n;
var totalAmount = arr.reduce(summingReducer, 0)
totalAmount; // 6
```

equals

```
const arr = [1, 2, 3];
var totalAmount = arr.reduce((acc, n) => acc + n, 0)
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

IMPORTANT: `=>` lacks its own `this` and `arguments`. Also it can't be used as a constructor.

Old way with `arguments`:

```
var sum = function(){
	var args = [].slice.call(arguments)
	  .reduce(function(a, b){ return a + b })
}
sum(1, 2, 3, 4) //=== 10
```

With arrow function:

`var sum = (...numbers) => numbers.reduce((a, b) => a + b)`

### Default parameter values
```
const orZero = (n = 0) => n;
orZero(); // 0
orZero(2); // 2
orZero(undefined); // 0
```

### Parameter destructuring with ES6
```
function makeSound({species = 'animal', sound}) {
   console.log('The ' + species + ' says ' + sound + '!');
}

makeSound({
   weight:23,
   sound: 'woof'
})
makeSound({
   species: 'bird',
   weight:23,
   sound: 'chirp'
})
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

### Recursion
ES5 has a call stack limit! ES6 has removed it!

```
let categories = [
   {id: 'animals', parent: null},
   {id: 'mammmals', parent: 'animals'},
   {id: 'cats', parent: 'mammmals'},
   {id: 'dogs', parent: 'mammmals'},
   {id: 'chihuahua', parent: 'dogs'},
   {id: 'labrador', parent: 'dogs'},
   {id: 'siamese', parent: 'cats'},
   {id: 'persian', parent: 'cats'},
]

let makeTree = (categories, parent) => {
   let node = {};
   categories
      .filter(c => c.parent === parent)
      .forEach(c => node[c.id] = makeTree(categories, c.id));
   return node;
}

console.log(
   JSON.stringify(makeTree(categories, null), null, 2)
);
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
Maybe to complicated to be used because of easily made errors! Avoid when possible.

### Function composition
```
const inc = n => n + 1;
inc(double(2)); // 5
```

The value `2` is passed into `double()`, which produces `4`. `4` is passed into `inc()` which evaluates to `5`.

You can pass any expression as an argument to a function. The expression will be evaluated before the function is applied:

`inc(double(2) * double(2)); // 17`

Since `double(2)` evaluates to `4`, you can read that as `inc(4 * 4)` which evaluates to `inc(16)` which then evaluates to `17`.

#### compose(), be careful
```
const add1 = n => n + 1;
const double = n => n * 2;
const add1ThenDouble = compose(
  double,
  add1
);
add1ThenDouble(2); // 6
// ((2 + 1 = 3) * 2 = 6)
```

So be aware, `compose()` evaluates right-to-left!! A solution to this is:

`const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);`

Now you can write add1ThenDouble() like this:

```
const add1ThenDouble = pipe(
  add1,
  double
);
add1ThenDouble(2); // 6
// ((2 + 1 = 3) * 2 = 6)
```

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

const censor = words => filter(word => word.length !== 4, words);
const startsWithS = words => filter(word => word.startsWith('g'), words);

const wordArray = ['oops', 'gasp', 'shout', 'sun'];
console.log(censor(wordArray)); // [ 'shout', 'sun' ]
console.log(startsWithS(wordArray)); // [ 'gasp' ]
```

Source: https://medium.com/javascript-scene/higher-order-functions-composing-software-5365cf2cbe99

### Pure functions
Pure functions depend only on the inputs of the function, and the output should be the exact same for the same input.

```
// pure function
const add10 = (a) => a + 10

// impure function due to external non-constants
let x = 10
const addx = (a) => a + x

// also impure due to side-effect
const setx = (v) => x = v
```

Use whenever possible, because they are clean and have no side-effects to worry about.

## Monads, Functors, and Fancy Words
Monads can be thought of as a container for a value, and to open up the container and do something to the value, you need to map over it. Array.filter() is a functor/monad!. Here’s a simple example:

```
const inc = n => n + 1;
const isZero = n => n === 0;
// monad
list = [-1,0,1]
list.map(inc) // => [0,1,2]
list.map(isZero) // => [false, true, false]
```

```
list.map(inc).map(isZero) // => [true, false, false]
list.map(compose(isZero, inc)) // => [true, false, false]
```

See: https://medium.com/javascript-scene/functors-categories-61e031bac53f

## "Classes", Factories, inheritance and it's best practices
JavaScript has no classes! Even the ES6 `class` desugars to constructor functions! Use them only if unavoidable.

See:
https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e
https://medium.com/javascript-scene/3-different-kinds-of-prototypal-inheritance-es6-edition-32d777fa16c9

### Factory function (use if possible!)
```
const proto = {
  hello () { return `Hello, my name is ${ this.name }`; }
};

const greeter = (name) => Object.assign(Object.create(proto), {name});

const george = greeter('george');
const msg = george.hello();
console.log(msg); // Hello, my name is George
```

### ES5 constructor function (don't use if possible)
```
function Greeter (name) {
  this.name = name || 'John Doe';
}

Greeter.prototype.hello = function hello () {
  return 'Hello, my name is ' + this.name;
}

var george = new Greeter('George');
var msg = george.hello();
console.log(msg); // Hello, my name is George
```

### ES6 class (don't use if possible)
```
class Greeter {
  constructor (name) {
    this.name = name || 'John Doe';
  }
  hello () { return `Hello, my name is ${ this.name }`; }
}

const george = new Greeter('George');
const msg = george.hello();
console.log(msg); // Hello, my name is George
```

### Inheritance
However inheritance comes to life (factory functions, constructor functions or "classes"), don't think in terms of IS-A relationships, but in terms of HAS-A, USES-A or CAN-DO relationships (composition!).

### Concatenative Inheritance / Cloning / Mixins
Concatenative inheritance is the process of copying the properties from one object to another, without retaining a reference between the two objects.

```
const proto = {
  hello: function hello() { return `Hello, my name is ${ this.name }`; }
};

const george = Object.assign({}, proto, {name: 'George'});
const msg = george.hello();
console.log(msg); // Hello, my name is George
```

### Functional Inheritance
The primary advantage of using functions for extension is that it allows you to use the function closure to encapsulate private data. In other words, you can enforce private state.

```
const rawMixin = function () {
  const attrs = {}; // -> get's private!!

  return Object.assign(this, {
    set (name, value) {
      attrs[name] = value;

      this.emit('change', {
        prop: name,
        value: value
      });
    },

    get (name) {
      return attrs[name];
    }
  }, WhateverElseToApply);
};

const mixinModel = (target) => rawMixin.call(target);

const george = { name: 'george' };
const model = mixinModel(george);

model.on('change', data => console.log(data));

model.set('name', 'Sam');
```

### !!! Composition !!!
```
const barker = (state) => ({
   bark: () => console.log('Woof, I'am ' + state.name');
})
const driver = (state) => ({
   drive: () => state.position = state.position + state.speed;
})
const killer = (state) => ({
   kill: () => console.log('Astalavista, baby!');
})

const murderRobotDog = (name) => {
   let state = {
      name,
      speed: 100,
      position: 0
   }
   return Object.assign(
      {},
      barker(state),
      driver(state),
      killer(state)
   )
}

murderRobotDog('sniffles').bark(); // Woof, I'm sniffles
```

#### Composition with Stamps (stampit.js)
```
const a = init(function () {
  const a = 'a';

  Object.assign(this, {
    getA () { return a; }
  });
});

const b = init(function () {
  const a = 'b';

  Object.assign(this, {
    getB () { return a; }
  });
});

const c = compose(a, b); // Stampit functionality

const foo = c();
console.log(foo.getA()); // 'a'
console.log(foo.getB()); // 'b'
```

```
// Some more privileged methods, with some private data.
const availability = init(function () {
  let isOpen = false; // private

  Object.assign(this, {
    open () {
      isOpen = true;
      return this;
    },
    close () {
      isOpen = false;
      return this;
    },
    isOpen () {
      return isOpen;
    }
  });
});

// Here's a stamp with public methods, and some state:
const membership = compose({
  methods: {
    add (member) {
      this.members[member.name] = member;
      return this;
    },
    getMember (name) {
      return this.members[name];
    }
  },
  properties: {
    members: {}
  }
});

// Let's set some defaults:
const defaults = compose({
  properties: {
    name: 'The Saloon',
    specials: 'Whisky, Gin, Tequila'
  }
});

const overrides = init(function (overrides) {
  Object.assign(this, overrides);
});

// Classical inheritance has nothing on this. No parent/child coupling.
// No deep inheritance hierarchies.
// Just good, clean code reusability.
const bar = compose(availability, membership, defaults, overrides);
const myBar = bar({name: 'Moe\'s'});

// Silly, but proves that everything is as it should be.
const result = myBar.add({name: 'Homer'}).open().getMember('Homer');
console.log(result); // { name: 'Homer' }
console.log(`
  name: ${ myBar.name }
  isOpen: ${ myBar.isOpen() }
  specials: ${ myBar.specials }
`);
/*
  name: Moe's
  isOpen: true
  specials: Whisky, Gin, Tequila
*/
```


# Examples of nice functional programming

## The greeting mess

We need to greet a user on login. Easy:

`const greeting = (name) => `Hello ${name}`

But wait ... we need more greeting text:

```
const greeting = (name, male=false, female=false) =>
  `Hello ${male ? ‘Mr. ‘ : female ? ‘Ms. ‘ : ‘’} ${name}`
```

Now it starts getting messier, and we still need moooore greeting text! What now? More parameters, more ifs?

No, just write and compose pure functions!

```
const formalGreeting = (name) => `Hello ${name}`;
const casualGreeting = (name) => `Sup ${name}`;
const male = (name) => `Mr. ${name}`;
const female = (name) => `Mrs. ${name}`;
const doctor = (name) => `Dr. ${name}`;
const phd = (name) => `${name} PhD`;
const md = (name) => `${name} M.D.`;

formalGreeting(male(phd("Chet"))); // => "Hello Mr. Chet PhD"
```

This is a nice approach, but a litte unhandy in use. So lets do this:

```
const pipe = (fns) => (x) => fns.reduce((v, f) => f(v), x); // a helper for 'chaining' functions

const identity = (x) => x; // Simply returns the input value
const greet = (name, options) => {
   return pipe([
      // greeting    
      options.formal ? formalGreeting :
      casualGreeting,

      // prefix
      options.doctor ? doctor :
      options.male ? male :
      options.female ? female :
      identity,

      // suffix
      options.phd ? phd :
      options.md ?md :
      identity
   ])(name)
};
```

Yeah! Thats nice, readable, extensible and good code! :)
