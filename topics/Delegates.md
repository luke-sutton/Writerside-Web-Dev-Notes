# Delegates

Note:
: Delegates are no longer widely used and have been replaced with Func and Action.

A delegate is a type whose instances hold a reference to a method with a particular parameter list and return
type.

```C#
/*Declare a delegate that can encapsulate a method that takes a
 string as an argument and returns void*/
public delegate void Callback(string message);

// Create a method for a delegate -
public static void DelegateMethod(string message)
{
    Console.WriteLine(message);
}

// Instantiate the delegate -
Callback handler = DelegateMethod;

// Call the delegate -
handler("Hello World");
```

We can store more than one function in one variable of delegate type -

```C#
Print print1 = text => Console.WriteLine(text.ToUpper());
Print print2 = text => Console.WriteLine(text.ToLower());
print printmulticast = print1 + print2;

multicast("Hello");

delegate void Print(string input);

/* output -
Hello
hello */
```

These are referred to as multicast delegates.

You can also chain multiple variables of delegate type using +=

```C#
Print print1 = text => Console.WriteLine(text.ToUpper());
Print print2 = text => Console.WriteLine(text.ToLower());
Print printmulticast = print1 + print2;
Print print3 = text => Console.WriteLine(text.Substring(0,3));
multicast += print3;

multicast("Hello");

delegate void Print(string input);

/* output -
Hello
hello
Hel */
```
