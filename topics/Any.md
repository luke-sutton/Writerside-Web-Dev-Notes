# Any

Note:
:  Any is used to check if any element in a collection matches the given criteria.

Example -

```C#
var numbers = new[] { 5, 9, 2, 12, 6};
bool isAnyLargerThan10 = numbers.Any(number => number > 10);
```

In the above example the isAnyLargerThan10 function iterates through the array of numbers and returns true if
any of them are larger than 10.

You can also use it to check if any elements exist in a collection -

```C#
var isNotEmpty = numbers.Any();
```

This will return true if the numbers collection is not empty, and return false if it is.