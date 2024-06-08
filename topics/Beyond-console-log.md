# Beyond console.log

There are many ways of capturing information in the console.

These different types can be sorted in the browser console to make it
easier to find the required information.

### console.log

This is the main method used that displays the provided data to the console -

```Javascript
console.log("Hello, World!");
```

### console.error

These are distinguished as errors and are printed in red making them stand out -

```Javascript
console.error("This is an error message");
```

### console.warn

These are distinguished as warnings and are printed in amber -

```Javascript
console.warn("This is a warning message");
```

### console.info

These are distinguished as information in the console -

```Javascript
console.info("This is an informational message");
```

### console.debug

These are just for debugging purposes and are often hidden by default.
To display them ensure that verbose is selected in the console settings.

```Javascript
console.debug("This is a debug message");
```

### console.table

console.table() allows you to display data as a table. This can be very
helpful when working with arrays or objects.

```Javascript
const users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 },
];

console.table(users);
```

### console.time() and console.timeEnd()

These methods are used to measure the time taken for a piece of code to execute.

```Javascript
function processData() {
  console.time("processData");
  // Simulating a time-consuming process
  for (let i = 0; i < 1000000; i++) {
    // Process
  }
  console.timeEnd("processData");
}

processData();
```

### console.count

console.count() is used to count the number of times a code block is executed.

```Javascript
for (let i = 0; i < 5; i++) {
  console.count("Counter");
}
```

### console.group() and console.groupEnd()

These methods allow you to group together multiple log messages.

```Javascript
console.group("User Details");
console.log("Name: Alice");
console.log("Age: 25");
console.groupEnd();
```

### console.assert

console.assert() writes a message to the console only if an assertion is false.

```Javascript
const x = 10;
console.assert(x > 10, "x is not greater than 10");
```

In the above example since x is not greater than 10, the message will be printed.

### Styling Console Logs

You can style console log messages using CSS.

```Javascript
console.log("%cThis is a styled message", "color: blue; font-size: 20px;
```