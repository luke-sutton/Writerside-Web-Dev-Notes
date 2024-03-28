# Enums

Note:
: Enum is short for "enumerations", which means "specifically listed".

An enum is a special "class" that represents a group of constants.

To create an enum, use the enum keyword, and separate the enum items with a comma.

```C#
enum Level 
{
  Low,
  Medium,
  High
}
```

You can access enum items with the dot syntax:

```C#
Level myVar = Level.Medium;
Console.WriteLine(myVar);
```

### Enums in a Switch Statement

Enums are often used in switch statements to check for corresponding values:

```C#
enum Level 
{
  Low,
  Medium,
  High
}

Level myVar = Level.Medium;

switch(myVar) 
{
case Level.Low:
  Console.WriteLine("Low level");
  break;
case Level.Medium:
   Console.WriteLine("Medium level");
  break;
case Level.High:
  Console.WriteLine("High level");
  break;
}
```