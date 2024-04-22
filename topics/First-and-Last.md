# First and Last

Note:
: Returns the first and last elements of a collection

When used without any arguments, First returns the first element of the collection, and Last returns
the last

```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

int first = numbers.First();  // first would be 1
int last = numbers.Last();  // last would be 5
```

First and Last can also take a predicate (function that returns true or false) to find the first or
last element that satisfies certain conditions -

```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

int firstEven = numbers.First(n => n % 2 == 0);  // firstEven would be 2
int lastOdd = numbers.Last(n => n % 2 != 0);  // lastOdd would be 5
```

In this example, First returns the first even number, and Lst the last odd number.  
The lamda expressions n => n % 2 and n => n % 2 != 0 are the predicates that define the conditions.

```C#
public class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Name = "Alice", Age = 20 },
    new Student { Name = "Bob", Age = 21 },
    new Student { Name = "Charlie", Age = 22 }
};

var firstStudentOlderThan21 = students.First(s => s.Age > 21);

Console.WriteLine(firstStudentOlderThan21.Name); 
// Output: Charlie
```

The above example will return the name of the first student who is older than 21.

First and Last will throw an exception if there is no element in the collection that satisfies the condition.

### FirstOrDefault & LastOrDefault

These are very similar to First and Last but instead of throwing an exception if no suitable element is found, it
returns the default value for the type (null for reference types and zero for numeric value types).

```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

int firstEvenGreaterThanTen = numbers.FirstOrDefault(n => n % 2 == 0 && n > 10);
Console.WriteLine(firstEvenGreaterThanTen);  // Output: 0
```