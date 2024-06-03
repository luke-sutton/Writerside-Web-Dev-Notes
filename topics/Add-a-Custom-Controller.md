# Add a Custom Controller

Duplicate the WeatherForecastController and make a new one called UserController.   
Rename the class to the same name (UserController).   
Delete the array, model and contents of the method.    
change the route to "test".   
Change the methods return type to a string array and its name to Test.

This should leave you with the following code -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class UserController : ControllerBase
{
    [HttpGet("test")]
    public string[] Test()
    {
        
    }
}
```

Within the method create a new string array called responseArray and return it -

```C#
string[] responseArray = new string[]
{
    "test1",
    "test2"
};
return responseArray;
```

You can now run the application and the new endpoint should successfully return the array of strings.

### Add a Parameter

Next we will add a parameter to the endpoint, add a string called testValue as a parameter -

```C#
[HttpGet("test")]
public string[] Test(string testValue)
{
    string[] responseArray = new string[]
    {
        "test1",
        "test2"
    };
    return responseArray;
}
```

Make the parameter explicit by adding it to the route -

```C#
[HttpGet("test/{testValue}")]
```

Parameters should be made explicit as it makes it much more obvious to the user as to what is required.

Using the parameter -

add the argument to the string array being returned -

```C#
string[] responseArray = new string[]
{
    "test1",
    "test2",
    testValue
};
return responseArray;
```

Now if you call the endpoint and pass in a string, that string will be returned in the results.

Note:
: The IActionResult return type represents the return value from an API call, it is a catch-all type
that can be used to return any result from an API call. Alternatively the actual type returned can be used instead.

