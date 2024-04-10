# Tuples

a Tuple is a container that can store multiple values of different data types.  
It is defined using angle brackets (<>) containing the data types for each element.

The most basic way to create a Tuple in C# is by using the Tuple class, as shown below -

```C#
Tuple<int, string> myTuple = new Tuple<int, string>(1, "One");
```

You can also create a new tuple using Tuple.Create without needing to explicitly name the types -

```C#
var person = Tuple.Create(1, "One");
```

In C# 7.0 and later, you can create a Tuple using new syntax using parentheses.  
This is how you would create it with explicit ty;e declaration -

```C#
(var id, var name) = (1, "One");
```

You can also create a Tuple using an implicit type declaration -

```C#
var myTuple = (1, "One");
```

### Accessing Tuple Elements

You access individual tuple elements using dot notation -

```C#
int age = myTuple.Item1;
string name = myTuple.Item2;
```

### Tuples in Classes and Functions

You can use tuples in classes and methods using generic types syntax (angle brackets) -

```C#
return new Tuple<int, int>(min, max)
```

Example of a simple function that takes 2 tuples as parameters and returns the 2 tuples swapped -

```C#
public static Tuple<T2, T1> SwapTupleItems<T1, T2>(Tuple<T1, T2> resultTuple)
{
    return new Tuple<T2, T1>(resultTuple.Item2, resultTuple.Item1);
}
```