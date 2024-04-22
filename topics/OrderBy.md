# OrderBy

The OrderBy method is used to sort collections in ascending order.

```C#
List<int> numbers = new List<int> { 5, 7, 2, 4, 3, 9 };

var sortedNumbers = numbers.OrderBy(n => n);
```

In the above example the OrderBy method sorts the numbers in ascending order.  
The lamda expression (n => n) is used to specify that we want to sort the numbers by their own value.

### OrderByDescending

To order by descending you can use OrderByDescending() like this -

```C#
var sortedNumbersDescending = numbers.OrderByDescending(n => n);
```

### Ordering Complex Types

The OrderBy methods also work with complex types and allows you to specify the property to sort by -

```C#
public class Employee
{
    public string Name { get; set; }
    public int Age { get; set; }
}

List<Employee> employees = new List<Employee>
{
    new Employee { Name = "Steve", Age = 50 },
    new Employee { Name = "Bill", Age = 55 },
    new Employee { Name = "Lucy", Age = 30 }
};

var sortedEmployees = employees.OrderBy(e => e.Name);
```

This would sort the employees by their name in ascending order.

### ThenBy

ThenBy allows you to sort by further properties once it has been sorted by the initial property.

```C#
using System;
using System.Linq;
using System.Collections.Generic;

public class Employee
{
    public string Department { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}

public class Program
{
    public static void Main()
    {
        List<Employee> employees = new List<Employee>
        {
            new Employee { Department = "HR", Name = "Steve", Age = 50 },
            new Employee { Department = "HR", Name = "Bill", Age = 55 },
            new Employee { Department = "Marketing", Name = "Lucy", Age = 30 },
            new Employee { Department = "HR", Name = "Anna", Age = 40 },
            new Employee { Department = "Marketing", Name = "Mike", Age = 35 }
        };

        var sortedEmployees = employees.OrderBy(e => e.Department).ThenBy(e => e.Name);

        foreach (var employee in sortedEmployees)
        {
            Console.WriteLine(employee.Department + ", " + employee.Name + ", " + employee.Age);
        }
    }
}
```

In the above example, the employees are sorted by Department using OrderBy. Then within each department,
they are sorted by their name alphabetically.