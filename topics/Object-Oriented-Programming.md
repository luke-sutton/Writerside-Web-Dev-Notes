# Object-Oriented Programming

in C# objects are created with blueprints called classes.  
classes should be named with a capital letter.  

Classes consist of-  
Fields / Properties  
Constructors  
Methods

```C#
public class Rectangle
{
    // Properties
    public int Width { get; set; }
    public int Height { get; set; }

    // Constructor
    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }

    // Methods
    public int CalculateCircumference()
    {
        return 2 * (Width + Height);
    }

    public int CalculateArea()
    {
        return Width * Height;
    }
}
```