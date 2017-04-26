# JS-Cheat-Sheet
Hopefully best practices in modern JS.

## Variable declarations
### var, let & const
`const` is the most strict declaration and can't be reassigned. It should be used as default whereever possible.

`let` is the second strict declaration and can be reassigned. It should be used for iteration counter.

`var` is the less strict declaration and should be avoided when possible.


Once variables with `const` or `let` are declared, any attemp to declare them again will fail.

## Types
### Arrays
An ordered list of values.

`[1, 2, 3];` -> The literal notation.
`const arr = [1, 2, 3];` -> With a given name.
