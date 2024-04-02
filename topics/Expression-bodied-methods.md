# Expression-bodied methods

Expression-bodied methods are a shorter syntax for writing methods.

In order to use them the method must consist of a single expression or statement, and have the same return type
as the methods return type or return void.

To use these you remove the curly brackets and the return keyword, and add => after the parameter brackets of the method.

Note:
: => is most often referred to as a fat arrow.

For example -
```C#
 public int CalculateCircumference()
{
    return 2 * (Width + Height);
}
```

Can be shortened to -
```C#
 public int CalculateCircumference() => 2 * (Width + Height);
```