# Add Endpoints

### Move Endpoints to a Separate File

Add a new folder called Endpoints then add a new class file with an appropriate name for your endpoints (e.g
AddressBookEndpoints)

Make the class static then add a static method with the name Configure + your class name (e.g.
ConfigureAddressBookEndpoints), and pass in WebApplication -

```C#
namespace MinimalAPI_test.Endpoints;

public static class AddressBookEndpoints
{
    public static void ConfigureAddressBookEndpoints(this WebApplication app)
    {
    }
}
```

Then in the Program.cs we need to invoke the new class by calling it on the app method -

```C#
app.ConfigureAddressBookEndpoints();
```

This is usually placed just before app.UseHttpsRedirection()

### Add an Endpoint

To keep the code as clean as possible we will separate the logic from the endpoint by moving it to a separate method.

In your endpoints class file, add an endpoint to the method we previously created -

```C#
namespace MinimalAPI_test.Endpoints;

public static class AddressBookEndpoints
{
    public static void ConfigureAddressBookEndpoints(this WebApplication app)
    {
        app.MapGet("/api/addressbook", GetAllAddressBook);
    }
}
```

In the above example we are creating a get endpoint by calling MapGet on the app object, and we are passing in the url
for the endpoint, and then the name of the method we would like to call at that endpoint. This method is where we 
will hold our logic, but we have not created this yet, so select the method name and select create method, and the 
method will be created for us -

```C#
namespace MinimalAPI_test.Endpoints;

public static class AddressBookEndpoints
{
    public static void ConfigureAddressBookEndpoints(this WebApplication app)
    {
        app.MapGet("/api/addressbook", GetAllAddressBook);
    }

    private static Task GetAllAddressBook(HttpContext context)
    {
        throw new NotImplementedException();
    }
}
```

Now we can update this function for our needs. Start by making the method async, then update the return type, and then
pass in our repository. Finally, add the required logic -

```C#
private static async Task<IResult> GetAllAddressBook(IAddressBookRepository _repo)
{
    return await _repo.GetAllAsync();
}
```

We should now be able to run the application and successfully call the endpoint.

### Add an HTTP Response Code

We can return an appropriate HTTP response code with the result by wrapping the return object in a Results method.
For example, if we wrap our result in Results.Ok, a 200 OK status code will be returned along with the results -

```C#
private static async Task<IResult> GetAllAddressBook(IAddressBookRepository _repo)
{
    return Results.Ok(await _repo.GetAllAsync());
}
```

Here are the response codes that can be returned -

Results.Ok(result): Responds with a 200 OK status code and includes the result in the body of the response.

Results.Created(uri, result): Returns a 201 status code indicating that a new resource was successfully created. The uri
parameter is a string that contains the URI of the new resource and result is the new resource which you want to send
along in the response.

Results.CreatedAtRoute(routeName, routeValues, result): (This can be used when you have a named route) Returns a 201
status code, indicating that a new resource was successfully created. The routeName parameter is a string that contains
the name of the route to generate the URI of the new resource. routeValues is an object that contains the route values
used to generate the URI. The result parameter is the new resource to include in the body of the response. The method
also adds a Location header to the response indicating the URI of the newly created resource.

Results.NoContent(): Returns a 204 status code indicating that the operation was successful and there's no additional
information to send in the response.

Results.BadRequest(error): Returns a 400 status indicating that the server could not understand the request due 
to invalid syntax. You can provide additional error information.

Results.NotFound(error): Returns a 404 status code indicating that the requested resource could not be found on the
server.

Results.UnprocessableEntity(error): Returns a 422 status code indicating the server understands the content type of the
request entity, and the syntax of the request entity is correct, but it was unable to process the contained
instructions.

### Named Endpoints

You can name endpoints using the WithName method and by passing in the name you want to give the endpoint -

```C#
public static void ConfigureAddressBookEndpoints(this WebApplication app)
{
    app.MapGet("/api/addressbook", GetAllAddressBook).WithName("GetAddressBooks");
}
```

This name can be used later if you have to refer to this route.

### Produces and Accepts

We can further improve our API response by using the Produces and Accepts methods to show what data the endpoint accepts
and what it returns. These are added to the end of the endpoint and can be chained together -

```C#
app.MapPost("/api/addressbook", CreateAddressBook).WithName("CreateAddressBook").Accepts<AddressBook>
            ("application/json").Produces<AddressBook>(201).Produces(400);
```

In the above example, we are stating that the post endpoint accepts an AddressBook model in JSON format, and it 
can return either an AddressBook object with a successfully created status code (201) or can return a unsuccessful
code (400) if there was an error.

Now when a user looks at our endpoint is postman or swagger etc. they will have a much clearer view of what
they need to supply and what they can expect to be returned.

### ILogger

We can use ILogger to enable logging by passing it into, and then calling it from the endpoint method -

```C#
private static async Task<IResult> GetAllAddressBook(IAddressBookRepository _repo, ILogger<Program> _logger)
{
    _logger.Log(LogLevel.Information, "Getting all address books");
    return Results.Ok(await _repo.GetAllAsync());
}
```

### Data Transfer Objects (DTO)

When making a post request to create a new entry, the request body as shown in swagger or postman will use our model
that contains the 'id' field. This field does not need to be provided as it is just used for indexing and is
created and handled by the database.

To stop this from showing in our request body we can create a new model with that field removed. This is referred to
as a Data Transfer Object (DTO).

Create a new folder within the Models folder and call it DTO, then add a new class file for our model. These models
are usually named with the object name followed by its purpose followed by DTO (e.g. AddressBookCreateDTO).
Then just copy the parameters across from the existing model with the unneeded property removed -

```C#
namespace MinimalAPI_test.Models.DTO;

public class AddressBookCreateDTO
{
    public string? FirstName { get; set; }
    public string? Surname { get; set; }
    public int Age { get; set; }
    public string? Address { get; set; }
}
```

Now in the endpoint method, we can pass in this new model instead of the existing one, and then within the method
we can create a new object using these values, and use this when creating an entry -

```C#
private static async Task<IResult> CreateAddressBook(IAddressBookRepository _repo,
    AddressBookCreateDTO addressBookCreateDTO, ILogger<Program> _logger)
{
    _logger.Log(LogLevel.Information, "Creating an address book");

    var addressBook = new AddressBook
    {
        FirstName = addressBookCreateDTO.FirstName,
        Surname = addressBookCreateDTO.Surname,
        Age = addressBookCreateDTO.Age,
        Address = addressBookCreateDTO.Address
    };

    await _repo.CreateAsync(addressBook);
    await _repo.SaveAsync();

    return Results.Created($"/api/addressbook/{addressBook.Id}", addressBook);
}
```