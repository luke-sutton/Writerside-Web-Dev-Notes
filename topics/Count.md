# Count

Note:
:  Count returns the number of elements in a collection that matches the provided criteria.

Example -

```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

int count = numbers.Count(n => n > 3);

Console.WriteLine($"There are {count} numbers greater than 3."); // Outputs: There are 2 numbers greater than 3.
```

You can also use it without any provided criteria to simply count the number of elements in the collection -

```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

int count = numbers.Count();

Console.WriteLine($"The list contains {count} elements."); // Outputs: The list contains 5 elements.
```

### LongCount

If the expected result is larger than the maximum size of an int, then the LongCount method should be used -

```C#
long count = numbers.LongCount();

Console.WriteLine($"The list contains {count} elements.");
```