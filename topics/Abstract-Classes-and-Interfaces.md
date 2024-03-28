# Abstract Classes & Interfaces

Note:
: When to use which - As a general rule, you should use an abstract class when creating a base class that needs to be inherited
by other classes in a class hierarchy. If you need to define a behavior that can be implemented by multiple
unrelated classes, you should use an interface.

### Abstract Classes

Abstract classes are base classes that cannot be used to create classes, they are only created to be derived from.

They are created using the abstract modifier keyword.

They can contain constructors.

They can contain both abstract and regular methods.

Abstract methods can only be used in abstract classes and must be overridden in the derived class.

```C#
abstract class Animal
{
    // Abstract method
    public abstract animalSound() 
    
    // Regular method
    public void sleep()
    {
        Console.WriteLine("Zzz");
    }
}

public class Pig : Animal
{
    public override void animalSound()
    {
        Console.WriteLine("Oink");
    }
}

Pig myPig = new Pig(); // Create a Pig object
myPig.animalSound();  // Call the abstract method
myPig.sleep();  // Call the regular method
```

### Interfaces

An interface is similar to an abstract class, but whereas an abstract class may contain method definitions,
fields, and constructors, an interface may only have declarations of events, methods, and properties.

They should be named using PascalCase and should begin with a capital I. (e.g. IShape)

Derived classes must implement all methods declared in the interface.

Classes can implement more than one interface, they should be listed separated with a comma.

```C#
// Interface
interface IAnimal 
{
  void animalSound(); // interface method (does not have a body)
}

// Pig "implements" the IAnimal interface
class Pig : IAnimal 
{
  public void animalSound() 
  {
    // The body of animalSound() is provided here
    Console.WriteLine("The pig says: wee wee");
  }
}

Pig myPig = new Pig();  // Create a Pig object
myPig.animalSound();
```