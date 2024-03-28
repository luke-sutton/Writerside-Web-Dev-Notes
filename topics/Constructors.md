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