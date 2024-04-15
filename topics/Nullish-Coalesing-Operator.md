# Nullish Coalesing Operator (??)

The nullish coalescing operator (??) is a logical operator that returns its right-hand side
operand when its left-hand side operand is null or undefined, and otherwise returns its left-hand side operand. 

Examples:

```Javascript
let age = 0;
let defaultAge = 18;
let userAge = age ?? defaultAge;

// output userAge = 0;

let age = null;
let defaultAge = 18;
let userAge = age ?? defaultAge;

// output userAge = 18;
```