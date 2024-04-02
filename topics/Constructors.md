# Constructors

Constructors set the initial values for created objects.  
They should be named using PascalCase and have the same name as the class to which they belong.  

```C#
public class Rectangle
{
    public int Width { get; set; }
    public int Height { get; set; }

    #This is the constructor
    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }
}
```

In order to create an object using the above class you would declare a new instance of it -  

```C#
Rectangle rectangle = new(5, 10);
```

In the above example the constructor of the Rectangle class is being called with the values 5 and 10.  
The constructor assigns these values to the width and height fields using the properties of the class.

### Constructor Overloading

Multiple constructors can be created for a class, they should have the same name and be differentiated by
the properties being passed in.

```C#
public class Rectangle
{
    public string Name { get; set; }
    public int Width { get; set; }
    public int Height { get; set; }

    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }
    
    public Rectangle(int width, int height, string name)
    {
        Width = width;
        Height = height;
        Name = name;
    }
}
```

In the above example if a Rectangle class is created with two integers the first constructor will be called.  
If the class is created with two integers and a string the 2nd will be called.  

It is also possible to call one constructor from another using the 'this' keyword.

```C#
public class Rectangle
{
    public string Name { get; set; }
    public int Width { get; set; }
    public int Height { get; set; }

    public Rectangle(int width, int height) : this(width, height, "default name")
    {
    }
    
    public Rectangle(int width, int height, string name)
    {
        Width = width;
        Height = height;
        Name = name;
    }
}
```

In the above reworked example as the duplicated code in the constructors has been removed.  
If a Rectangle class is created with just two integers the first class is called, this then calls
the 2nd constructor passing in the provided integers as well as a default name for the string parameter.

### Optional Parameters

It is also possible to have parameters that are optional and to have a default value set for them.

Default Parameters must be placed after the required ones.

Using the previous example we can shorten the code further using default parameters -

```C#
public class Rectangle
{
    public string Name { get; set; }
    public int Width { get; set; }
    public int Height { get; set; }
    
    public Rectangle(int width, int height, string name = "default name")
    {
        Width = width;
        Height = height;
        Name = name;
    }
}
```

This allows us to remove one of the constructors, this will have the same effect that a Rectangle object can
be created with only the width and height values provided and it will be given a default name.