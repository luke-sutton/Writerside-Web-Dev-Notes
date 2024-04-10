# Lamda Expressions

Lamda Expressions are also known as anonymous functions, are another shorthand for writing functions.

The syntax looks very similar to that used to create expression-bodied methods except they do not have a name.

```C#
(input-parameters) => {statements}
```

Lamda expressions are mostly used when a method needs to be passed as a parameter into another method.

For example -
```C#
static bool isTen(int n) {
  return n == 10;
}

Array.Exists(numbers, isTen);
```

Can be shortened by placing the function logic as a lamda as a 2nd parameter -
```C#
Array.Exists(numbers, (int n) => {
  return n == 10;
});
```

This can be shortened further to -

```C#
Array.Exists(numbers, n => n == 10);
```

The following code is from the notes on using the Func delegate -

```C#
var numbers = new[] { 1, 4, 7, 19 ,2};

IsLargerThan10(numbers, IsLargerThan10);
IsEvan(numbers, IsEven)

bool IsAny(IEnumerable<int> numbers, Func<int,bool> predicate)
{
    foreach (var number in numbers)
    {
        if (pridicate(number))
        {
            return true;
        }
    }
    return false;
}

bool IsLargerThan10(int number)
{
    return number > 0;
}

bool IsEven(int number)
{
    return number % 2 == 0;
}
```

This code can be simplified using lamda expressions as the 2nd parameter, allowing the
separate functions to be removed -

```C#
var numbers = new[] { 1, 4, 7, 19 ,2};

IsAny(numbers, n => n > 10);
IsAny(numbers, n => n % 2 ==0);

bool IsAny(IEnumerable<int> numbers, Func<int,bool> predicate)
{
    foreach (var number in numbers)
    {
        if (pridicate(number))
        {
            return true;
        }
    }
    return false;
}
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