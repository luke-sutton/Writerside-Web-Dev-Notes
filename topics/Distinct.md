# Distinct

Note:
: Removes all duplicated values form a collection

Ensures that a collection only contains unique elements.

```C#
var numbers = new[] { 1, 2, 2, 3, 4, 4, 5, 6, 6, 7 };

var distinctNumbers = numbers.Distinct();
```

In the above example as there are duplicates in the numbers variable, these will be removed in distinctNumbers.