# First Endpoint

Here is an example of how to make a very basic get endpoint in a minimal API.

First call the method MapGet on the app -

```C#
app.MapGet();
```

THen pass in the url pattern as a string, then a lamda function with no parameters that returns a string -

```C#
app.MapGet("/helloworld", () => "Hello World");
```

And that's it, you can now run the app, and going to the address helloworld, will show are response of "Hello World".

The different request types can be made by calling the appropriate Map method, for example a post request can be made
using MapPost -

```C#
app.MapPost("/helloworld2", () => "Hello World 2");
```

To run multiple lines of code / calculations in the call just add a method body with a return statement -

```C#
app.MapGet("/helloworld", () =>
{
    return "Hello World";
});
```

### Return Types

Results is a factory class that provides shortcuts for generating ActionResults in a .NET app.

* Results.ok()

This returns as http 200 status code, indicating that the request was successful -

```C#
app.MapPost("/helloworld2", () => Results.Ok("Hello World 2"));
```

* Results.NotFound()

This returns an HTTP 404 status code, indicating that the requested resource could not be found.

* Results.BadRequest()

This returns an HTTP 400 status code, indicating that the server couldn't understand the request due to invalid syntax.

```C#
app.MapGet("/helloworld", () =>
{
    return Results.BadRequest("Exception!");
});
```

### Route Parameters

Route parameters should be added to the end of the route pattern as usual, then added to the lambda function
as a parameter. The parameter can then be used in the response -

```C#
app.MapGet("/helloworld/{id:int}", (int id) =>
{
    return Results.Ok("ID:" + id);
});
```