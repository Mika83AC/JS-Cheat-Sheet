<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [JS-Cheat-Sheet](#js-cheat-sheet)
  - [Variable declarations](#variable-declarations)
    - [var, let & const](#var-let--const)
    - [Shorthands with ES6 destructuring](#shorthands-with-es6-destructuring)
    - [Block-scoping](#block-scoping)
    - [Hoisting](#hoisting)
  - [Closures](#closures)
  - [this](#this)
  - [Comparison](#comparison)
  - [Ternaries (IF shorthand)](#ternaries-if-shorthand)
  - [Iterators](#iterators)
  - [Arrays](#arrays)
    - [Helpful array methods](#helpful-array-methods)
  - [Switch](#switch)
  - [eval() and with](#eval-and-with)
  - [Objects](#objects)
    - [Prototypes](#prototypes)
      - [Delegate Prototypes](#delegate-prototypes)
    - [Propertie mutation/replacement](#propertie-mutationreplacement)
      - [Prototype Cloning](#prototype-cloning)
    - [Object creation](#object-creation)
      - [Constructor functions](#constructor-functions)
      - [Factories](#factories)
      - [Factory Inheritance](#factory-inheritance)
      - [Prototypal Inheritance with Stamps](#prototypal-inheritance-with-stamps)
    - [Composing objects](#composing-objects)
  - [Functions](#functions)
    - [ES6 arrow functions](#es6-arrow-functions)
    - [Parameter detault values](#parameter-detault-values)
    - [Parameter destructuring with ES6](#parameter-destructuring-with-es6)
    - [Named parameters](#named-parameters)
    - [Parameters of unknown length](#parameters-of-unknown-length)
    - [Named Function Expressions](#named-function-expressions)
    - [Pure functions / Stateless functions](#pure-functions--stateless-functions)
    - [Recursion](#recursion)
    - [Rest and Spread](#rest-and-spread)
    - [Currying](#currying)
    - [Partial Application](#partial-application)
    - [Function composition (needs check if examples really function composition)](#function-composition-needs-check-if-examples-really-function-composition)
      - ['Piping' functions together](#piping-functions-together)
    - [Method Chaining](#method-chaining)
    - [Higher-Order-Functions](#higher-order-functions)
  - [Monads, Functors, and Fancy Words](#monads-functors-and-fancy-words)
  - ["Classes", Factories, inheritance and it's best practices](#classes-factories-inheritance-and-its-best-practices)
    - [Factory function (use if possible!)](#factory-function-use-if-possible)
    - [ES5 constructor function (don't use if possible)](#es5-constructor-function-dont-use-if-possible)
    - [ES6 class (don't use if possible)](#es6-class-dont-use-if-possible)
    - [Inheritance](#inheritance)
    - [Concatenative Inheritance / Cloning / Mixins](#concatenative-inheritance--cloning--mixins)
    - [Functional Inheritance](#functional-inheritance)
    - [Composition](#composition)
      - [Composition with Stamps (stampit.js)](#composition-with-stamps-stampitjs)
  - [Asynchronous Operations](#asynchronous-operations)
    - [Callbacks](#callbacks)
    - [Promises and Deferreds](#promises-and-deferreds)
  - [Modules](#modules)
    - [Node-style Modules](#node-style-modules)
    - [ES6 Modules](#es6-modules)
  - [Interfaces](#interfaces)
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

### Shorthands with ES6 destructuring
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

### Block-scoping
Important for the Garbage Collector to work effectiv.

```
function process(data) {
	// do something interesting
}

// anything declared inside this block can go away after! But only does, if block-scoped! Otherwise it will stay because the click function has a closure over the entire scope!
{
	let someReallyBigData = { .. };
	process( someReallyBigData );
}

var btn = document.getElementById( "my_button" );
btn.addEventListener("click", function click(evt) {
	console.log("button clicked");
});
```

### Hoisting
Declaration always pulled up to top of scope, but assignment always takes in place.

```
a = 2;
var a;
console.log( a ); // 2
```
```
console.log( a ); // undefined
var a = 2;
```

## Closures
Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.

The most common closure mistake:
```
for (var i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}

// Results in '6' printed out 5 times!
```

## this
Determining the `this` binding for an executing function requires finding the direct call-site of that function. Once examined, four rules can be applied to the call-site, in this order of precedence:

1. Called with `new`? Use the newly constructed object.

2. Called with `call` or `apply` (or `bind`)? Use the specified object.

3. Called with a context object owning the call? Use that context object.

4. Default: undefined in `strict mode`, global object otherwise.

Be careful of accidental/unintentional invoking of the default binding rule. In cases where you want to "safely" ignore a `this` binding, a "DMZ" object like `ø = Object.create(null) is` a good placeholder value that protects the `global` object from unintended side-effects.

Instead of the four standard binding rules, ES6 arrow-functions use lexical scoping for `this` binding, which means they adopt the `this` binding (whatever it is) from its enclosing function call. They are essentially a syntactic replacement of `self = this` in pre-ES6 coding.

## Comparison
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

## Iterators
Try to use higher-order-functions (`.forEach()`, `.reduce()` ...) instead of `for()` loops. This will avoid much boilerplate and bugs.

## Arrays
An ordered list of values.

`const arr = [2, 4, 6];`

### Helpful array methods
```
arr.filter(x => x > 2); // [4, 6]

arr.map(x => x * 2); // [4, 8, 12]

arr.reduce((acc, n) => acc + n, 0); // 10

// arr itself gets never mutated, always a new array is returned
```

More: https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Array

## Switch
Simple to miss the `break;` statement which leads to difficult to find bugs.

So better exchange `switch`es with:
```
function doAction(action) {
  var actions = {
    'hack': function () {
      return 'hack';
    },
    'slash': function () {
      return 'slash';
    },
  };

  if (typeof actions[action] !== 'function') {
    throw new Error('Invalid action.');
  }

  return actions[action]();
}
```

## eval() and with
Simply don't use them because it can be a security problem (`eval()`) and leads to poor performance.

## Objects
In JavaScript, all types of functions, arrays, key/value pairs, and data structures in general are really objects.

### Prototypes
A prototype is an object intended to model other objects after. It is similar to a class in that you can use it to construct any number of object instances, but different in the sense that it is an object itself. There are two ways that prototypes can be used: you can delegate access to a single, shared prototype object (called a delegate), or you can make clones of the prototype.

#### Delegate Prototypes
In JavaScript, objects have an internal reference to a delegate prototype. When an object is queried for a property or method, the JavaScript engine first checks the object. If the key doesn't exist on that object, it checks the delegate prototype, and so on up the prototype chain. The prototype chain typically ends at the Object prototype.

#### Propertie mutation/replacement
```
var colorProto = {
   setColorName: function setColorName(newColor) {
      this.colorName = newColor;
      return this;
   },
   colorName: 'white',
   rgb: { r: 255, g: 255, b: 255 }
},
white = Object.create(colorProto),
red = Object.create(colorProto).setColorName('red');

white.colorName; //white
red.colorName; //red

red.rgb.r = 0; // also sets white.rgb.r to 0 -> Object and array mutations are shared!
red.rgb = {r: 255, g: 0, b: 0}; // only sets red.rgb to 255,0,0 -> Property replacement is instance-specific!
```

#### Prototype Cloning
See http://chimera.labs.oreilly.com/books/1234000000262/ch03.html#prototype_cloning, but there seems to be a mistake in the example. I don't get it.

### Object creation

#### Constructor functions
Don't use.

#### Factories
A factory is a method used to create other objects and avoids the drawbacks of constructor functions. Its purpose is to abstract the details of object creation from object use. In object-oriented design, factories are commonly used where a simple class is not enough.

```
var carPrototype = {
   accelerate: function accelerate(amount) {
      this.mph += amount;
      return this;
   },
   brake: function brake(amount) {
      this.mph = ((this.mph - amount) < 0)? 0 : this.mph - amount;
      return this;
   },
   color: 'pink',
   direction: 0,
   mph: 0
},

car = function car(options) {
   return Object.assign(Object.create(carPrototype), options);
},

myCar = car({color: 'red'});
```

#### Factory "Inheritance"
Not sure if this is a 'best practice'!

```
var BeingProto = {
   // Members
   name: '',
   alive: true,

   // Methods
   die: function die() {
      this.alive = false;
      console.log(this.name + ' died');
      return this;
   }
}
var BeingFactory = function BeingFactory(options) {
   return Object.assign(Object.create(BeingProto), options);
}

var AnimalProto = {
   // Members
   walking: false,

   // Methods
   walk: function walk() {
      this.walking = true;
      console.log(this.name + ' walks');
      return this;
   },
   rest: function rest() {
      this.walking = false;
      console.log(this.name + ' rests');
      return this;
   },
   kill: function kill(beeing) {
      console.log(this.name + ' kills the ' + beeing.name);
      beeing.die();
   }
}
var AnimalFactory = function AnimalFactory(options) {
   return Object.assign(Object.create(AnimalProto), BeingProto, options);
}


var antilope = AnimalFactory({name: 'Antilope'});
antilope.walk();

var tiger = AnimalFactory({name: 'Tiger'});
tiger.walk();
tiger.kill(antilope);
tiger.rest();
```

#### Prototypal Inheritance with Stamps
Claims to combine the best of all object creation variants (delegate prototype, instance state, encapsulation).

See: https://github.com/stampit-org/stampit

### Composing objects
Objects can be easily composed together into new objects with `Object.assign()`.

```
const oA = { a: 'a' };
const oB = { b: 'b' };
const c = Object.assign({}, oA, oB); // c becomes { a: 'a', b: 'b' }
```

Note that when you use `Object.assign()`, you must pass a destination object as the first parameter. It is the object that properties will be copied to. If you forget, and omit the destination object, the object you pass in the first argument will be mutated.

## Functions
### ES6 arrow functions
```
const double = x => x * 2; //ES6 arrow function
const double = x => { return x * 2; } //ES6 arrow function

const double = function(x) { return x * 2; } // Old way
```

IMPORTANT: `=>` lacks its own `this` and `arguments`. Also it can't be used as a constructor.

### Parameter detault values
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

### Named parameters
```
var userProto = {
   name: '',
   email: '',
   alias: '',
   showInSearch: true,
   colorScheme: 'light'
};

function createUser(options) {
   return Object.assign({}, userProto, options);
}

var newUser = createUser({
   name: 'Mike',
   showInSearch: true
});
newUser;
```

### Parameters of unknown length
```
var sum = (...numbers) => numbers.reduce((a, b) => a + b)
sum(1, 2, 3, 4) // 10

var sumOldWay = function(){
	var args = [].slice.call(arguments)
	  .reduce(function(a, b){ return a + b })
}
sumOldWay(1, 2, 3, 4) // 10
```

### Named Function Expressions
For better debugging, don't create anonymous functions at top-levels as this will be hard to stacktrace:
```
// don't
var lightbulbAPI = {
    off: function() {},
    on: function() {},
};

// do
var lightbulbAPI = {
    off: function off() {},
    on: function on() {},
};
```

### Pure functions / Stateless functions
Pure or stateless functions do not use or modify variables, objects, or arrays that were defined outside the function. So they don't depend on anything else as the input and behave always the same.

```
// pure function
const add10 = (a) => a + 10

// impure function due to external non-constants
let x = 10
const addx = (a) => a + x

// also impure due to side-effect
const setx = (v) => x = v
```

```
// impure, because it changes the input variable
var rotate = function rotate(arr) {
  arr.push(arr.shift());
  return arr;
}

//pure, because it returns a new object instead changing the input
var safeRotate = function safeRotate(arr) {
  var newArray = arr.slice(0);
  newArray.push(newArray.shift());
  return newArray;
}
```

That feature is particularly useful in JavaScript applications, because you often need to manage a lot of asynchronous events. Consequently, time becomes a major factor in code organization.

Because you don't have to worry about clobbering shared data, stateless functions can often be run in parallel, meaning that it's much easier to scale computation horizontally across a large number of worker nodes. In other words, stateless functions are great for high-concurrency applications.

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

### Partial Application
```
var multiply = (x, y) => x * y;
var boundDouble = multiply.bind(null, 2); // null context
boundDouble(4): // 8
```

The only disadvantage is that you won't be able to override the value of this with .call() or .apply(). If your function uses this, you shouldn't use .bind().

### Function composition (needs check if examples really function composition)
```
const inc = n => n + 1;
const double = n => n * 2;
inc(double(2)); // 5
```

You can pass any expression as an argument to a function. The expression will be evaluated before the function is applied:
`inc(double(2) * double(2)); // 17`

#### 'Piping' functions together
`const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);`

Now you can write add1ThenDouble() like this:
```
const add1ThenDouble = pipe(add1, double);
add1ThenDouble(2); // ((2 + 1 = 3) * 2 = 6)
```

### Method Chaining
```
const arr = [1, 2, 3];
arr.map(double).map(double); // [4, 8, 12]
```

### Higher-Order-Functions
How JavaScripts `Array.filter()` is build:
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

## Monads, Functors, and Fancy Words
Monads can be thought of as a container for a value, and to open up the container and do something to the value, you need to map over it. `Array.filter()` is a functor/monad!. Here’s a simple example:

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
  hello: function hello() {
     console.log(`Hello, my name is ${ this.name }`)
  }
};

const greeter = (name) => Object.assign(Object.create(proto), {name});

const george = greeter('George');
george.hello(); // Hello, my name is George
```

### ES5 constructor function (don't use if possible)
```
function Greeter (name) {
  this.name = name || 'John Doe';
}

Greeter.prototype.hello = function hello() {
  console.log(`Hello, my name is ${ this.name }`)
}

var george = new Greeter('George');
george.hello(); // Hello, my name is George
```

### ES6 class (don't use if possible)
```
class Greeter {
  constructor (name) {
    this.name = name || 'John Doe';
  }
  hello: function hello() {
     console.log(`Hello, my name is ${ this.name }`)
  }
}

const george = new Greeter('George');
george.hello(); // Hello, my name is George
```

### Inheritance
However inheritance comes to life (factory functions, constructor functions or "classes"), don't think in terms of IS-A relationships, but in terms of HAS-A, USES-A or CAN-DO relationships which leads you to composition instead of parent-child inheritance.

### Concatenative Inheritance / Cloning / Mixins
Concatenative inheritance is the process of copying the properties/functions from one object to another, without retaining a reference between the two objects.

```
const proto = {
  hello: function hello() {
     console.log(`Hello, my name is ${ this.name }`)
  }
};

const george = Object.assign({}, proto, {name: 'George'});
george.hello(); // Hello, my name is George
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

### Composition
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

## Asynchronous Operations
### Callbacks
Callbacks are functions that you pass as arguments to be invoked when the callee has finished its job. Callbacks are commonly passed into event handlers, Ajax requests, and timers.

### Promises and Deferreds
Better examples needed.

## Modules
Modules in the browser use a wrapping function to encapsulate private data in a closure (for example, with an IIFE; see “Immediately Invoked Function Expressions”). Without the encapsulated function scope provided by the IIFE, other scripts could try to use the same variable and function names, which could cause some unexpected behavior.

This approach works everywhere, but can only load "all or nothing":
```
var app = {};

// Module Storage
(function (exports) {

   (function (exports) {
      var api = {
         moduleStorage: function test() {
            console.log('I\'m the storage module.');
         }
      };

      Object.assign(exports, api);
   }((typeof exports === 'undefined') ? window : exports));

}(app));

// Module Picture
(function (exports) {

   (function (exports) {
      var api = {
         modulePicture: function test() {
            console.log('I\'m the picture module.');
         }
      };

      Object.assign(exports, api);
   }((typeof exports === 'undefined') ? window : exports));

}(app));

app.moduleStorage();
app.modulePicture();
```

Here, that variable is called `exports`, for compatibility with CommonJS (see “Node-Style Modules” for an explanation of CommonJS). If exports does not exist, you can fall back on `window`:

### Node-style Modules
Maybe the best because most popular until ES6 modules arrive in the wild.
```
'use strict';
var foo = function foo () {
  return true;
};

exports.foo = foo;
```

Then use require() (require.js) to import your module and assign it to a local variable. You can specify the name of a module in the list of installed Node modules or specify a path to the module using relative paths.

### ES6 Modules
Watch closely when they will become supported! Best approach if it will be available.

## Interfaces
Interfaces are one of the primary tools of modular software design. Interfaces define a contract that an implementing module will fulfill. For instance, a common problem in JavaScript applications is that the application stops functioning if the Internet connection is lost. In order to solve that problem, you could use local storage and sync changes periodically with the server. Unfortunately, some browsers don't support local storage, so you may have to fall back to cookies or even Flash (depending on how much data you need to store).


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
