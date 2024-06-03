# Convert Initial App To Controller Structure

Now we will convert the default application to use a controller structure.

First create a new folder called Controllers,   
Then create a new file within called WeatherForecastControllers.

Note:
: The name given to the controller is important as it needs to match the name of the route with controller added
to the end.   
As our route is /weatherforcast the file must be called WeatherForecastController.

Add the appropriate namespace and class name to the file (if not auto created) -

```C#
namespace DotNetAPI.Controllers;

public class WeatherForecastController
{
    
}
```

Next add the APIController attribute above the class -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
public class WeatherForecastController
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
public class WeatherForecastController : ControllerBase
{
    
}
```

Next connect the route by adding the Route attribute and passing in "[controller]"

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    
}
```

This is used to define the route that the API responds to. The word controller is a placeholder for your
class name minus the controller suffix. So as our class is called WeatherForecastController, the
[Route("[controller]")] attribute defines that this controller will handle HTTP
requests sent to <app_root>/weatherforecast.

That is the main blueprint for the controller route created, so now we just need to move the functionality
from the program.cs file to here.

First create add a new public method that returns an IEnumerable of type WeatherForecast
and call it GetFiveDayForecast -

```C#
public IEnumerable<WeatherForecast> GetFiveDayForecast()
{
    
}
```

Next move the code from within the MapGet delegate from program.cs and place it into the new method -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    public IEnumerable<WeatherForecast> GetFiveDayForecast()
    {       
        var forecast = Enumerable.Range(1, 5).Select(index =>
                new WeatherForecast
                (
                    DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                    Random.Shared.Next(-20, 55),
                    summaries[Random.Shared.Next(summaries.Length)]
                ))
            .ToArray();
        return forecast;
    }
}
```

Then take the WeatherForecast model (record) and add it to the bottom of the file, and make it public -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{   
    public IEnumerable<WeatherForecast> GetFiveDayForecast()
    {
        var forecast = Enumerable.Range(1, 5).Select(index =>
                new WeatherForecast
                (
                    DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                    Random.Shared.Next(-20, 55),
                    _summaries[Random.Shared.Next(_summaries.Length)]
                ))
            .ToArray();
        return forecast;
    }
}

public record WeatherForecast(DateOnly Date, int TemperatureC, string? Summary)
{
    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);
}
```

And then take the summaries variable and add it to the beginning of the class -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    var summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public IEnumerable<WeatherForecast> GetFiveDayForecast()
    {
        var forecast = Enumerable.Range(1, 5).Select(index =>
                new WeatherForecast
                (
                    DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                    Random.Shared.Next(-20, 55),
                    _summaries[Random.Shared.Next(_summaries.Length)]
                ))
            .ToArray();
        return forecast;
    }
}

public record WeatherForecast(DateOnly Date, int TemperatureC, string? Summary)
{
    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);
}
```

Then change it from a var to a private readonly string array, and add an underscore to the
beginning of its name (be sure to add the underscore to where it is called in the function as well) -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    private readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public IEnumerable<WeatherForecast> GetFiveDayForecast()
    {
        var forecast = Enumerable.Range(1, 5).Select(index =>
                new WeatherForecast
                (
                    DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                    Random.Shared.Next(-20, 55),
                    _summaries[Random.Shared.Next(_summaries.Length)]
                ))
            .ToArray();
        return forecast;
    }
}

public record WeatherForecast(DateOnly Date, int TemperatureC, string? Summary)
{
    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);
}
```

Finally we need to add a new attribute above the GetFiveDayForecast method to identify
that the type of request is a get, and the route to listen to.
We do this by adding the HttpGet attribute and pass in the route and optionally a name -

```C#
[HttpGet("", Name = "GetWeatherForecast")]
```

So this states that the method listens for http get requests from the controllers route, and the name
GetWeatherForecast is assigned to it.

Here is the completed controller -

```C#
using Microsoft.AspNetCore.Mvc;

namespace DotNetAPI.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    private readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    [HttpGet("", Name = "GetWeatherForecast")]
    public IEnumerable<WeatherForecast> GetFiveDayForecast()
    {
        var forecast = Enumerable.Range(1, 5).Select(index =>
                new WeatherForecast
                (
                    DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                    Random.Shared.Next(-20, 55),
                    _summaries[Random.Shared.Next(_summaries.Length)]
                ))
            .ToArray();
        return forecast;
    }
}

 public record WeatherForecast(DateOnly Date, int TemperatureC, string? Summary)
{
    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);
}
```

We now need to connect our controller to are program.cs file.

Within the program.cs file we can now remove the remnants of the MapGet delegate, so you should only have
the following -

```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
else
{
    app.UseHttpsRedirection();
}

app.Run();
```

Add the AddControllers service to the builder ath the top of the file -

```C#
builder.Services.AddControllers();
```

Then at the end of the file but before app.Run(), call the MapControllers method on the app -

```C#
app.MapControllers();
```

This will give the program access to our controller endpoints.

The final program.cs file should look like this -

```C#
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
else
{
    app.UseHttpsRedirection();
}

app.MapControllers();

app.Run();
```

