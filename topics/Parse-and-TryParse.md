# Parse & TryParse

### Parse

Parsing is a method to convert a string into another type.  

```C#
Parse.int("12");
```

The above example will convert the string "12" and converts it to the int 12.  

```C#
Parse.DateTime("Thursday, February 26, 2009");
```

The above example will convert the string into a DateTime object.

### TryParse

The TryParse returns a bool stating if parsing the input would be successful as well as the result or a default value
if unsuccessful.  
This is beneficial as if the parse method is unsuccessful it will result in an error.

```C#
var userInput = Console.ReadLine();

bool isParsingSuccessful = int.TryParse(userInput, out int number);
```

In the above example if the user entered a number string the value of isParsing successful would be true and the
value of the number variable will be set to the parsed entered value.  

If the user entered a value that could not be parsed to an int like a letter, then the value of isParsing Successful
would be false and the value of the number field will be set to a default value, which for an int would be 0.