# Fields and Properties

### Fields

Fields are the variables available within a class.  
They should be private and named using camelCase and begin with an underscore.

```C#
private DateTime _date;
```

There is in most cases no need to create fields, just create the properties and the compiler will create the relevant
fields and access methods in the background.

### Properties

Properties are methods that allow access to the private fields.  
They can set specific conditions for getting and setting the field values.  
They are most commonly public and should be named using PascalCase with a noun or adjective.  
They can be created quickly by typing the shortcut 'prop' then pressing tab.

```C#
Public string Name { get; set; };
```

The above is an example of syntactic sugar (a term to describe a feature in a language that lets you do
something more easily/with less typing involved, but doesn’t really add any new functionality to the
language that it didn’t already have)

In the above example the full code that will be created would look as follows -

```C#
private string _name;

public string Name
{
    get { return _name; }
    set { _name = value; }
}
```