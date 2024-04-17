# LINQ

Note:
: LINQ is short for Language Integrated Query

LINQ is a c# library that allows you to write simple and readable queries for manipulating and querying data.

It can not only be used for c# collections but also databases, web services, XML files etc, and the code
remains the same for all uses.

LINQ is now built in to modern version of c#, but if you are working on an older version you would import 
the namespace with - 

```C#
using System.Linq
```

One of the main advantages of LINQ is that it enables uou to week with data in a more abstract and efficient way,
you simply describe what you want to do with the data and let LINQ handle the rest.

### Method Chaining

LINQ queries except and receive the generic type IEnumerable&lt;T>, this means that LINQ queries can be chained
together as ones output can be received by the next -

```C#
var orderedOddNumbers = numbers
    .Where(number => number % 2 == 1)
    .OrderBy(number => number);
```

In the above example the result of the LINQ query - Where, is passed directly into the next LINQ query - OrderBy.

This allows you to build complex queries that are easy to read and understand.

### Deferred Execution

The result of LINQ queries are only calculated when they are required making them very memory and performance
efficient.

```C#
var shortWords = words.Where(word => word.Length < 3).ToList();
```
In the example above the shortWords variable is not the result it is the query, and the calculation will only
run when that variable is called, at this point the variable will become the result of the query.
