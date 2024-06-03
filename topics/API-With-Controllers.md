# API With Controllers

To convert the minimal APIs program.cs file to work with controllers the following changes should be made -

First the included endpoint and its associated variable and record should be removed.

Then the AddControllers method should be called on the builder.Services collection at the beginning of the application -

```C#
builder.Services.AddControllers();
```

Then add the end of the application just before run is called, MapControllers should be called on the app -

```C#
app.MapControllers();
```

The completed program.cs file should look like this (with https redirect and CORS policies implemented) -

```C#
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddCors((options) =>
{
    options.AddPolicy("DevCors",
        (corsBuilder) =>
        {
            corsBuilder.WithOrigins("http://localhost:4200", "http://localhost:3000", "http://localhost:8000")
                .AllowAnyMethod().AllowAnyHeader().AllowCredentials();
        });
    options.AddPolicy("ProdCors",
        (corsBuilder) =>
        {
            corsBuilder.WithOrigins("https://myProductionSite.com")
                .AllowAnyMethod().AllowAnyHeader().AllowCredentials();
        });
});

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseCors("DevCors");
    app.UseSwagger();
    app.UseSwaggerUI();
}
else
{
    app.UseCors("ProdCors");
    app.UseHttpsRedirection();
}

app.MapControllers();

app.Run();
```