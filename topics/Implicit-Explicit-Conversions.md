# Implicit &amp; Explicit Conversions

### Implicit Conversions

In C# type conversions can be implicit if the conversion is considered safe and lossless.

For example converting an integer to a decimal.

```C#
int integer = 10;
decimal b = integer;
```

In the above example the integer is converted from an integer to a decimal as the conversion is considered safe.

### Explicit Conversions

If the conversion is not considered safe and lossless the conversion must be made explicit.

The syntax for this is placing the required type in brackets before the variable name.

```C#
string myString = "5";
int myInt = (int)myString;
```

This is also referred to as type casting.