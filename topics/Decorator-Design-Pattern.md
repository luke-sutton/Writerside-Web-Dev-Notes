# Decorator Design Pattern

Tje Decorator design pattern is a structural pattern that allows adding new behaviour to an object 
dynamically, without affecting the behavior of other objects from the same class. This is done by wrapping the
original object with an object of a decorator class.

This helps to follow the open/closed principle.

Example using a Pizza object -
First we define a Pizza interface -

```C#
public interface IPizza
{
    string GetDescription();
    double GetCost();
}
```

Next we define a concrete Pizza class -

```C#
public class Margherita: IPizza
{
    public string GetDescription()
    {
        return "Margherita";
    }

    public double GetCost()
    {
        return 10.0;
    }
}
```
Next we define a PizzaDecorator abstract class that also implements IPizza -

```C#
public abstract class PizzaDecorator: IPizza
{
    protected IPizza pizza;

    public PizzaDecorator(IPizza pizza)
    {
        this.pizza = pizza;
    }

    public virtual string GetDescription()
    {
        return pizza.GetDescription();
    }

    public virtual double GetCost()
    {
        return pizza.GetCost();
    }
}
```

Next we create concrete decorators -

```C#
public class ExtraCheese : PizzaDecorator
{
    public ExtraCheese(IPizza pizza)
    : base(pizza)
    {
    }

    public override string GetDescription()
    {
        return pizza.GetDescription() + ", Extra Cheese";
    }

    public override double GetCost()
    {
        return pizza.GetCost() + 2.5;
    }
}

public class Anchovy : PizzaDecorator
{
    public Anchovy(IPizza pizza)
    : base(pizza)
    {
    }

    public override string GetDescription()
    {
        return pizza.GetDescription() + ", Anchovy";
    }

    public override double GetCost()
    {
        return pizza.GetCost() + 3.0;
    }
}
```

The final usage will look like this -

```C#
IPizza pizza = new Margherita();
pizza = new ExtraCheese(pizza);
pizza = new Anchovy(pizza);
Console.WriteLine($"Desc: {pizza.GetDescription()}, Cost: {pizza.GetCost()}");
```

In this example, the Decorator design pattern allows us to dynamically add ExtraCheese and Anchovy functionality
to the pizza object, without changing the Margherita class. The behaviour og the pizza object can be extended at
run-time depending on the decorators applied to it.