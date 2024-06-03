# Add a Controller

Create a folder called Controllers, then add an appropriately named file for your controller that 
ends with Controller (e.g. UserController.cs)

Add the appropriate namespace and class name to the file (if not auto created) -

```C#
namespace DotNetAPI.Controllers;

public class UserController
{
    
}
```

Next add the APIController attribute above the class -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
public class UserController
{
    
}
```

This will tell the framework that this is a controller used for an API, which will provide several
features that will improve development.

Next inherit from the ControllerBase class to be able to benefit from the functionality it provides -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
public class UserController : ControllerBase
{
    
}
```

Next connect the route by adding the Route attribute and passing in "[controller]"

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class UserController : ControllerBase
{
    
}
```

This is used to define the route that the API responds to. The word controller is a placeholder for your
class name minus the controller suffix. So as our class is called UserController, the
[Route("[controller]")] attribute defines that this controller will handle HTTP
requests sent to <app_root>/UserController.

That is the main blueprint for a controller created, all that is left is to add the required endpoints.

### Simple Example

Create a public method called GetUsers that has a return type of string array, then add the HttpGet attribute
with an endpoint of GetUsers -

```C#
[HttpGet("GetUsers")]
public string[] GetUsers(string testValue)
{

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

### Simple Example - Add a Parameter

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

Make the parameter explicit by adding it to the route in the HttpGet attribute -

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

After running the app and calling the endpoint and providing a string, the result should be the array of strings
including the one you provided.

Completed simple example -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class UserController : ControllerBase
{
    [HttpGet("GetUsers")]
    public string[] GetUsers(string testValue)
    {
        string[] responseArray = new string[]
        {
            "test1",
            "test2",
            testValue
        };
        return responseArray;
    }
}
```