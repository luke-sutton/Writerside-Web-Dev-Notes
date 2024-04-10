# List &amp; IEnumerable

### List

When to use:
:  When you need to perform operations that require adding, removing or modifying elements frequently.

A list is a collection of elements that allows direct access and manipulation of data elements using
an index. Unlike arrays, it shrinks and grows dynamically as you add or remove data.

```C#
List<int> numbersList = new List<int> {10, 15, 18, 21, 25};

numbersList.add(30); // Add a number to the list

numbersList.remove(10) // Remove a number from the list

bool hasTen = numbersList.Contains(10); // Check if a number is in the list

Console.WriteLine(numbersList[2]); // Direct access to the 3rd element in the list

numbersList.Insert(20, 3); // Insert 3 at the third position
```

### IEnumerable

When to use:
:  When you ony need to iterate over a collection and don't need to modify the elements.

IEnumerable is a memory efficient way of stepping through a collection of elements

How to create a new IEnumerable -

```C#
IEnumerable<string> nameCollection = new List<string> { "Name A", "Name B", "Name C", "Name D" };

```
Or a new IEnumerable can be created using an already existing collection -

```C#
List<int> numberList = new List<int>() {1, 2, 3, 4, 5 };

IEnumerable<int> numberEnumerable = numberList;
```

### IEnumerable vs List

IEnumerable is best when it comes to loading data, and List is best for manipulating data.

| Aspect | IEnumerable                                  | List                                                    |
|--------|----------------------------------------------|---------------------------------------------------------|
| Loading Data| Lazily loads each item when required         | Promptly loads all items at once                        |
|Accessing Data| Forward-only, one item at a time             | Direct and random access to any item                    |
|Best for| Large datasets due to its efficient resource usage | Small to medium datasets requiring frequent data manipulation |
