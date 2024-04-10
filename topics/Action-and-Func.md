# Action and Func

Action and Func are delegate types.

The last parameter in a Func is the return type and the preceding types are the required parameters.
If no return type is required (void) then Action should be used instead of Func -

```C#
Func<int, string, decimal> someFunc;
Action<string, string, bool> someAction;
```

In the above examples someFunc references a method that requires an int and string parameters and returns a decimal.
someAction references a method that requires 2 string parameters and a bool parameter and returns void.

In the following code an array of numbers is created and used in 2 different function calls -

```C#
var numbers = new[] { 1, 4, 7, 19 ,2};

IsAnyLargerThan10(numbers);
IsAnyEvan(numbers)

bool IsAnyLargerThan10(IEnumerable<int> numbers)
{
    foreach (var number in numbers)
    {
        if (number > 10)
        {
            return true;
        }
    }
    return false;
}

bool IsAnyEvan(IEnumerable<int> numbers)
{
    foreach (var number in numbers)
    {
        if (number % 2 == 0)
        {
            return true;
        }
    }
    return false;
}
```

The problem is that these functions are very similar, the only difference being the condition
in the if statement.  
We would need to pass in a function as the parameter for the 2 methods.

```C#
var numbers = new[] { 1, 4, 7, 19 ,2};

IsAny(numbers, IsLargerThan10);
IsAny(numbers, IsEven)

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

In the above code the only differing parts of the previous 2 functions have been moved into their
own functions (IsLargerThan10, IsEven).  
A second parameter of type Func has been added to the main function to allow these functions to be passed in.