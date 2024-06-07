# HTTPS Redirect and CORS

### HTTPS Redirect

The following line in the program.cs ensures that users are redirected to https -

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

### CORS

CORS stands for Cross Origin Resource Sharing, this is a mechanism implemented in web browsers to
mitigate the security risks involved in making HTTP requests to different domains (known as cross-domain requests).

Basically, if a website receives a request from a different domain in sees it as a security risk, the request
will be denied and an error will be thrown.   
But there are some cases where requests from different origins are legitimate and need to be allowed, such as your
API and your front end working together. So to allow this you need to implement a CORS policy.

Here is an example of a completed CORS policy, which should be added as a service to the program.cs file -

```C#
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
```

The code above features 2 CORS policies, one for dev and one for production.   
The dev policy ("DevCors") contains 3 http localhost addresses, these are the main one used by angular, react and vue.
When building your API you can just include the appropriate localhost for your frontend.   
The production policy should just contain the URL for your live front end application.

These policies then need to be applied in the HTTP request pipeline -

```C#
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
```