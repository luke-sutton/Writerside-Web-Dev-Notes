# Contains

Note:
:  The Contains method checks if a given element is present in a collection

Example -

```C#
List<string> fruits = new List<string> { "Apple", "Banana", "Cherry" };

bool containsBanana = fruits.Contains("Banana");

Console.WriteLine($"The list contains 'Banana': {containsBanana}"); // Outputs: The list contains 'Banana': True
```

In the above example the containsBanana method iterates through the supplied collection of words and
returns true if the given word of Banana is present.