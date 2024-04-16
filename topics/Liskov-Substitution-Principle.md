# Liskov Substitution Principle

Principle:
: Every subclass or derived class should be substitutable for their base or parent class.

Example:
Here we have a Bird base class and two derived classes FlyingBird and Penguin -

```C#
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("I am flying");
    }
}

public class FlyingBird : Bird
{
}

public class Penguin : Bird
{
    public override void Fly()
    {
        throw new InvalidOperationException("Penguins can't fly");
    }
}
```

In this example, a Penguin cannot fly. This is in violation of the Liskov Substitution Principle because
if we used a function like this -

```C#
public void MakeBirdFly(Bird bird)
{
    bird.Fly();
}
```

It would work well with a FlyingBird object, but it would throw an exception with a Penguin object -

```C#
Bird bird1 = new FlyingBird();
MakeBirdFly(bird1);  // works fine

Bird bird2 = new Penguin();
MakeBirdFly(bird2);  // throws exception
```

This breaks the principle because a Penguin object cannot properly substitute for a Bird object.

The principle suggests that a better design would be to move the Fly method into only those classes
that can actually fly -

```C#
// Base class
public class Bird
{
}

// Flying bird class with Fly method
public class FlyingBird : Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("I am flying");
    }
}

// Penguin class without Fly method
public class Penguin : Bird
{
}
```