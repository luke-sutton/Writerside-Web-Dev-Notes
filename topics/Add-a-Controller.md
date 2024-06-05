# Add a Controller

Create a folder called Controllers, then add an appropriately named file that should also end with 
Controller (e.g. UserController.cs)

Add the appropriate namespace and class name to the file (if not auto created) -

```C#
namespace DotNetAPI.Controllers;

public class UserController
{
    
}
```

Next add the APIController attribute above the class (this will add the appropriate namespace) -

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

### Test the Controller

Next we will add a simple get request endpoint to check that everything is working.

Create a public method called TestEndpoint that has a return type of string, then add the HttpGet attribute
with an endpoint of TestEndpoint.   
Then within this method create a string variable called response and assign it the value of "Test passed", then
return the response variable -

```C#
[HttpGet("TestEndpoint")]
public string TestEndpoint()
{
    string response = "Test passed";
    return response;
}
```

You can now run the application and the new endpoint should successfully return the string.