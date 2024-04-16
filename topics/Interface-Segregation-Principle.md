# Interface Segregation Principle

Principle:
: A client should never be forced to implement an interface that it doesn’t use, or
clients shouldn’t be forced to depend on methods they do not use.

This essentially means that it's better to have many specific interfaces rather than a single general-purpose one.

This principal aims to minimize the coupling between software elements and make a system easier to refactor,
change and deploy.

For example, here we have a Worker Interface with methods Work, Eat and Sleep -

```C#
public interface IWorker
{
    void Work();
    void Sleep();
    void Eat();
}
```

And a Robot class that implements this interface -

```C#
public class Robot : IWorker
{
    public void Work()
    {
        Console.WriteLine("Robot is working");
    }

    public void Sleep()
    {
        // Robots don't sleep!
        throw new NotImplementedException();
    }

    public void Eat()
    {
        // Robots don't eat!
        throw new NotImplementedException();
    }
}
```

In this example the Robot class is forced to implement Sleep and Eat methods that are not relevant for it.

To follow the principle we should separate the IWorker interface into smaller specific interfaces -

```C#
public interface IWork
{
    void Work();
}

public interface ISleep
{
    void Sleep();
}

public interface IEat
{
    void Eat();
}
```

Then we can have a Human class implementing all three interfaces and a Robot class implementing only IWork -

```C#
public class Human : IWork, IEat, ISleep
{
    public void Work()
    {
        Console.WriteLine("Human is working");
    }

    public void Sleep()
    {
        Console.WriteLine("Human is sleeping");
    }

    public void Eat()
    {
        Console.WriteLine("Human is eating");
    }
}

public class Robot : IWork
{
    public void Work()
    {
        Console.WriteLine("Robot is working");
    }
}
```

This way the classes are only implementing and dependent on interfaces that are necessary for them.