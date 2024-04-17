# All

Note:
:  All checks if all elements in a collection matches the given criteria.

Example -

```C#
var numbers = new[] { 5, 9, 2, 12, 6};
bool areAllLargerThan10 = numbers.All(number => number > 10);
```

In the above example the areAllLargerThan10 function iterates through the numbers collection and returns true
only if all the numbers are larger than 10.