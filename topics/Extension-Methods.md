# Extension Methods

An extension method in C# is a method that is added to an existing type without altering its source code.
These methods appear as if they were included in the type's definition. 

Extension methods are defined in a static class but with a special format. The first parameter
of the method uses the this keyword followed by the type that you want to extend:

Note:
: The naming convention for classes for extension methods are the type being extended followed by extensions.  
E.G. StringExtensions, IntExtensions

```C#
public static class StringExtensions
{
    public static string Reverse(this string input)
    {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
}
```

The code above shows a StringExtensions static class with a Reverse extension method. This method
takes a string, converts it to a char array, reverses the array, and finally creates a new string
from the reversed array.

You can now use this extension method on a string -

```C#
string s = "Hello world!";
string reversed = s.Reverse();
Console.WriteLine(reversed); // Output: "!dlrow olleH"
```