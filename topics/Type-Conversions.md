# Type Conversions

### Implicit Conversions

In C# type conversions can be implicit if the conversion is considered safe.

For example converting an integer to a decimal.

```C#
int integer = 10;
decimal b = integer;
```

In the above example the integer is converted from an integer to a decimal as the conversion is considered safe.

### Casting

A cast is explicit when the conversion isn't done implicitly by the compiler,
and then you must use the cast operator

The syntax for this is placing the required type in brackets before the variable name.

```C#
string myString = "5";
int myInt = (int)myString;
```

### Parse & TryParse

Parsing is a method to convert a string into another type.

```C#
Parse.int("12");
```

The above example will convert the string "12" and converts it to the int 12.

```C#
Parse.DateTime("Thursday, February 26, 2009");
```

The above example will convert the string into a DateTime object.

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

### Convert Method

The Convert class provides methods to convert between types where simple casting is not possible.
You should use convert if you're dealing with input whose types ypu don't know beforehand, or you are not certain
your cast would ba valid.

```C#
// Converting String to Int32
string strNumber = "12345";
int iNum = Convert.ToInt32(strNumber);
Console.WriteLine(iNum);  // Output: 12345

// Converting Double to Int32
double dNum = 123.45;
int iNum2 = Convert.ToInt32(dNum);
Console.WriteLine(iNum2);  // Output: 123

// Converting String to Double
string strFloat = "123.45";
double dNum2 = Convert.ToDouble(strFloat);
Console.WriteLine(dNum2);  // Output: 123.45

// Converting Char to Int32 (ASCII value)
char ch = 'A';
int intVal = Convert.ToInt32(ch);
Console.WriteLine(intVal);  // Output: 65

// Converting Int32 to Char
int asciiVal = 66;
char ch2 = Convert.ToChar(intVal);
Console.WriteLine(ch2);  // Output: 'B'

// Converting Boolean to String
bool b = true;
string strBool = Convert.ToString(b);
Console.WriteLine(strBool);  // Output: "True"
```
