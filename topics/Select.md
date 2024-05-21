# Select

Note:
:  Allows each element of a collection to be transformed

```C#
int[] numbers = new int[] { 1, 2, 3, 4, 5 };

var squaredNumbers = numbers.Select(num => num * num).ToArray();
```

The above example shows a numbers array transformed into a new array where all the values are squared.

The Select function is useful to not only manipulate values, but also transform types.

```C#
int[] numbers = new int[] { 1, 2, 3, 4, 5 };

var stringNumbers = numbers.Select(num => num.ToString()).ToList();
```

In the above example the values are transform from integers to strings.

The Select method also accepts an overload to provide an index of the source elements.

```C#
int[] numbers = new int[] { 1, 2, 3, 4, 5 };

var stringNumbers = numbers.Select((num, index) => $"{index+1}: {num}").ToList();
```

In the above example the overload is used to return a numbered list of the elements in th collection.