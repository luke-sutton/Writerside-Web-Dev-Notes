# Inheritance & Polymorphism

### Inheritance

It is possible for classes to inherit fields and methods from other classes.

The parent class that is being inherited from is referred to as the base class.  
The child class that inherits from the base class is referred to as the derived class.

To inherit from a class use the : symbol after the base class name and follow it with the name of the derived
class.

In the below example the Luke class inherits from the Person class

```C#
public class Person
{
    public int Age { get; set; }
    
    public void sayGreeting()
    {
        Console.WriteLine("Hello");
    }
}

public class Luke : Person
{
    public string Name = "Luke";
}
```

In this example after a new instance of the Luke class will have access to the Age property and the sayGreeting
method.

You can use the base keyword to access the base class from a derived class

```C#
base.sayGreeting();
```

Writing the above in the Luke class would call the sayGreeting method in the Person class.

### Polymorphism

Polymorphism means many forms, and it is used with inheritance to allow methods to perform different tasks.

For example using the previous code we could have the Luke class change the behaviour of the sayGreeting class
to change the greeting response.

In order for a method to be overridden you must use the access modifiers 'override' and 'virtual'.
The function that needs to be overridden should be given the virtual modifier and the one changing its behaviour
should be given the override modifier.

```C#
public class Person
{
    public int Age { get; set; }
    
    public virtual void sayGreeting()
    {
        Console.WriteLine("Hello");
    }
}

public class Luke : Person
{
    public string Name = "Luke";
    
    public overide void sayGreeting()
    {
        Console.WriteLine("Hello, my name is Luke");
    }
}

public class Paul : Person
{
    public string Name = "Paul";
    
}
```

In the above example if the sayGreeting function was called on a Luke object it would use that classes
version of the method. But if it was called on a Paul object it would use the method in the base class as it
does not have its own version of the method to override it.