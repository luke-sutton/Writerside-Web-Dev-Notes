# Dictionary

Dictionary is a collection of key value pairs.

Dictionary's, like lists are generic collections, but unlike lists they are parametrized by 2 types. One for the keys
and one for the values.

```C#
// Declare a new dictionary (both key and value of type string)
var countryToCUrrencyMapping = new Dictionary<string, string>();

// Add key value pairs to the dictionary (key is 1st parameter, value is 2nd)
countryToCurrencyMapping.Add("USA", "USD");
countryToCurrencyMapping.Add("India", "INR");
countryToCurrencyMapping.Add("Spain", "EUR");

// Use the key to lookup its value
Console.WriteLine("Currency in Spain is " +
    countryToCurrencyMapping["Spain"]);
    
/* output -
Currency in Spain is EUR */
```

You can also add a key value pain using indexing -

```C#
countryToCurrencyMapping["Poland"] = "PLN";
```

You must also use indexing to update an existing entry (this will update the value) -

```C#
countryToCurrencyMapping["Poland"] = "EUR";
```

Note:
: Each key in a dictionary must be unique

You can use a collection initializer to initialize the dictionary like this -

```C#
var countryToCurrencyMapping = new Dictionary<string, string>();
{
    ["USA"] = "USD",
    ["India"] = "INR",
    ["Spain"] = "EUR",
};
```

Or like this -

```C#
var countryToCurrencyMapping = new Dictionary<string, string>();
{
    { "USA", "USD" },
    { "India", "INR" },
    { "Spain", "EUR" },
};
```

The first method is generally the preferred one as it makes it clear you are working
with a dictionary.

### ContainsKey Method

Searching a dictionary for a key that does not exist will result in an error.  
You can check to make sure an entry exists using the ContainsKey method -

```C#
if(countryToCurrencyMapping.ContainsKey("Spain"))
{
    Console.WriteLine("Currency in Spain is " +
        countryToCurrencyMapping["Spain"]);
}
```

### Looping Through a Dictionary

When looping through a dictionary the variable used will also be a key value pair -

```C#
foeach(var countyCurrencyPair in countryToCurrencyMapping)
{
    Console.WriteLine(
        $"Country: {countyCurrencyPair.Key}, " +
        $currency: {countyCurrencyPair.Value});
}
```