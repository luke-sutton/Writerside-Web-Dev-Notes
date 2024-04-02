# Working with JSON

### Serialize

In order to convert a c# object into JSON you need to serialize it using the JsonSerializer.Serialize()
method.

```C#
var person = new Person
{
    Firstname = "John",
    LastName = "Smith",
    YearOfBirth = 1972
};

var personAsJson = JsonSerializer.Serialize(person);
```

### Deserialize

In order to convert a JSON object into a c# object you need to deserialize it using the JsonSerializer.Deserialize<>()
method.

The object class that the JSON should be converted to should be named within the angle brackets in the method.

```C#
var personJson = 
"{\"FirstName\":\"John\",\"LastName\":\"Smith\",\"YearOfBirth\":1972}";

var personFromJson = JsonSerializer.Deserialize<Person>(personJson);
```