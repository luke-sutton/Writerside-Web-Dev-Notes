# Lamda Expressions

Lamda Expressions are also known as anonymous functions, and are another shorthand for writing functions.

The syntax looks very similar to that used to create expression-bodied methods.

```C#
(input-parameters) => {statements}
```

Lamda expressions are mostly used when a method needs to be passed as a parameter into another method.

For example -
```C#
int[] numbers = { 3, 10, 4, 6, 8 };
static bool isTen(int n) {
  return n == 10;
}

Array.Exists(numbers, isTen);
```

Can be shortened to -
```C#
Array.Exists(numbers, (int n) => {
  return n == 10;
});
```

### Shorter Lamda Expressions

There are multiple ways to shorten the concise lambda expression syntax.

The parameter type can be removed if it can be inferred.  
The parentheses can be removed if there is only one parameter.

```C#
int[] numbers = { 7, 7, 7, 4, 7 };

Array.Find(numbers, (int n) => { return n != 7; });

/* The type specifier on `n` can be inferred based on the array
 being passed in and the method body. */
Array.Find(numbers, (n) => { return n != 7; });

// The parentheses can be removed since there is only one parameter.
Array.Find(numbers, n => { return n != 7; });

/* Finally, we can apply the rules for expression-bodied methods
 (remove the return keyword and curly brackets). */
Array.Find(numbers, n => n != 7);
```