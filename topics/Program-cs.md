# Program.cs

After creating a new Web API application using .net 8 a Program.cs file will be created which holds
the main part of the application -

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

app.UseHttpsRedirection();

var summaries = new[]
{
    "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
};

app.MapGet("/weatherforecast", () =>
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
    })
    .WithName("GetWeatherForecast")
    .WithOpenApi();

app.Run();

record WeatherForecast(DateOnly Date, int TemperatureC, string? Summary)
{
    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);
}
```

The following line ensures that users are redirected to https -

```C#
app.UseHttpsRedirection();
```

This is great for security in a production environment, but can cause headaches when developing.

In order to handle this, moving the line into an else statement below the IsDevelopment check,
will ensure that it is only applied in production -

```C#
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
else
{
    app.UseHttpsRedirection();
}
```

The code after this line is the endpoint created in the default application (the default API only has a single get
endpoint).

This is a new structure introduced in .net 8.   
This can be usefull for testing or creating very small API's quickly by keeping all the code in this file, but 
for most API's it makes sense to move these to another location and use the controller infrastructure.