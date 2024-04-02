# Generic Types

Generics are used to specify type parameters for classes and functions at the time they are called or created.

For example a class with a generic type could be called with a parameter of type int or string and the generic
parameter will become this type.

### Generic Classes

Generic classes are defined using a type parameter in angle brackets after the class name. 
The following defines a generic class.

```C#
class DataStore<T>
{
    public T Data { get; set; }
}
```

The letter T in the example above is called a type parameter.  
In the example above data is a generic property because we have used a type parameter T as its
type instead of the specific data type.

Note:
: The letter T does not need to be used it is just common practice when there is just one parameter.

More than one generic type can be used as parameters and these should be separated with a comma.

```C#
class DataStore<TKey, TValue>
```

Note:
: When more than one generic type is used it is common practice to give them appropriate name with a
T at the beginning.