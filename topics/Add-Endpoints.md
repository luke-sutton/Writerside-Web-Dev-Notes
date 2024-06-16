# Add Endpoints

### Move Endpoints to a Separate File

To keep the project clean and manageable, and to reduce the amount of code within the program.cs file, it is a good
idea to move the endpoints code into their own file.

Add a new folder called Endpoints, then add a new class file with an appropriate name for your endpoints (e.g
AddressBookEndpoints)

Make the class static, then add a static method with the name Configure + your class name (e.g.
ConfigureAddressBookEndpoints), then pass in WebApplication -

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

In the above example we are creating a get all endpoint by calling MapGet on the app object, and we are passing in the
URI for the endpoint, and then passing the name of the method we would like to call at that endpoint. 

If we are making a get single call instead og get all the URI should include a placeholder for the identifier (e.g. an
id) that the user will provide -

```C#
app.MapGet("/api/addressbook/{id:int}", GetAddressBookById);
```

The method we passed in is where we will hold our logic, but we have not created this yet, so select the method name
and select create method, and the method will be created for us -

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

Now we can update this function for our needs. Start by making the method async, then update the return type to return
a Task of type IResult, and then pass in any required arguments. Finally, add the required logic -

```C#
private static async Task<IResult> GetAllAddressBook(IAddressBookRepository _repo)
{
    return await _repo.GetAllAsync();
}
```

As the above example is a get all request, and we are not providing any data, the only argument required is an instance
of our repository interface (which contains the logic for contacting the database) then we just call the appropriate
method from within our repository, which in this case is GetAllAsync.

When data is needed for a get single request (where the identifier will be part of the URI), the data type for the
identifier also needs to be passed in -

```C#
app.MapGet("/api/addressbook/{id:int}", GetAddressBookById);

private static async Task<IResult> GetAddressBookById(IAddressBookRepository _repo, int id)
{
    return await _repo.GetAsync(id);
}
```

When data is needed to be provided for a post or put request, we need to also pass in the object type that the data
format will be in -

```C#
private static async Task<IResult> CreateAddressBook(IAddressBookRepository _repo, AddressBook addressBook)
{
    await _repo.CreateAsync(addressBook);
    await _repo.SaveAsync();
    return addressBook;
}
```

In the above example we are creating an AddressBook, so we provide an instance of it as an argument, then call the
CreateAsync method within our repository and pass the instance to it. The SaveAsync method is then called to ensure
the database is saved with the new entry. (note - some people prefer to create methods to handle both functions, such
as CreateAndSaveAsync)

Where does the data come from?:
: The minimal API understands that as this is a post request, data will be provided from the request body. Sometimes
you may see FromBody being passed as an argument to show this, but it is not required as the API is smart enough to
understand that this is where the data comes form.

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

For post requests a 201 (created successfully) status code is more appropriate, along with returning where the new
entry can be found -

```C#
private static async Task<IResult> CreateAddressBook(IAddressBookRepository _repo, AddressBook addressBook)
{
    await _repo.CreateAsync(addressBook);
    await _repo.SaveAsync();

    return Results.Created($"/api/addressbook/{addressBook.Id}", addressBook);
}
```

So after the Address Book is successfully added to whatever system or database is being used, this function returns
a Created result (HTTP status code 201) with a header indicating where to find the new Address Book (in this case
via "/api/addressbook/{addressBook.Id}") along with the actual Address Book object in the message body.

Here are the response codes that can be returned:
: Results.Ok(result): Responds with a 200 OK status code and includes the result in the body of the response.

: Results.Created(uri, result): Returns a 201 status code indicating that a new resource was successfully created. The uri
parameter is a string that contains the URI of the new resource and result is the new resource which you want to send
along in the response.

: Results.CreatedAtRoute(routeName, routeValues, result): (This can be used when you have a named route) Returns a 201
status code, indicating that a new resource was successfully created. The routeName parameter is a string that contains
the name of the route to generate the URI of the new resource. routeValues is an object that contains the route values
used to generate the URI. The result parameter is the new resource to include in the body of the response. The method
also adds a Location header to the response indicating the URI of the newly created resource.

: Results.NoContent(): Returns a 204 status code indicating that the operation was successful and there's no additional
information to send in the response.

: Results.BadRequest(error): Returns a 400 status indicating that the server could not understand the request due 
to invalid syntax. You can provide additional error information.

: Results.NotFound(error): Returns a 404 status code indicating that the requested resource could not be found on the
server.

: Results.UnprocessableEntity(error): Returns a 422 status code indicating the server understands the content type of the
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

Now when a user looks at our endpoint in postman or swagger etc. they will have a much clearer view of what
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

To stop this from showing in our request body we can create a new model with that field removed, and then map it to the
original model. This is referred to as a Data Transfer Object (DTO).

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

So what is happening is that we are asking the user to provide the fields required by our DTO model, but our database
requires the standard model, so we create a new instance of the original model, and then map our DTO model properties
to the matching ones in the original model. The other fields that exist only in the original model we can set ourselves
in code, or in the case of the Id property allow the database to handle for us.

Note:
: For put requests where we are updating a record, a DTO should be created that matches the original model including the
id field. This id field is how the API will know what entry needs to be updated.

### AutoMapper

Instead of manually mapping the properties from are DTO model to the original,
We can use a library called AutoMapper to do this for us.

First install the required Nuget package -

* AutoMapper

Create a new class file in the project and call it MappingConfig.cs, inherit from
the profile class to be able to use its mapping functions -

```C#
using AutoMapper;

namespace MinimalAPI_test;

public class MappingConfig : Profile
{

}
```

Add a constructor and within it create the maps by first calling CreateMap
with the types to be converted. For the mapping to work in both directions, 
call the ReverseMap method -

```C#
using AutoMapper;
using MinimalAPI_test.Models;
using MinimalAPI_test.Models.DTO;

namespace MinimalAPI_test;

public class MappingConfig : Profile
{
    public MappingConfig()
    {
        CreateMap<AddressBook, AddressBookCreateDTO>().ReverseMap();
    }
}
```

Now within the program.cs file we can use dependency injection to add our mapping config.
Add the AddAutoMapper service and pass in our config file -

```C#
builder.Services.AddAutoMapper(typeof(MappingConfig));
```

Now in our endpoint, pass in an instance of the mapper, and instead of mapping
the fields manually, we can call the Map function on the mapper object, specify the type
we want it assigned to and then pass in the object to be mapped -

```C#
private static async Task<IResult> CreateAddressBook(IAddressBookRepository _repo,
    AddressBookCreateDTO addressBookCreateDTO, ILogger<Program> _logger, IMapper _mapper)
{
    _logger.Log(LogLevel.Information, "Creating an address book");
    
    var addressBook = _mapper.Map<AddressBook>(addressBookCreateDTO);

    await _repo.CreateAsync(addressBook);
    await _repo.SaveAsync();

    return Results.Created($"/api/addressbook/{addressBook.Id}", addressBook);
}
```

This mapper recognizes fields of the same name within the 2 models and maps them accordingly.

### Validations

Validation logic can be added to your endpoints logic to validate the data provided.

When a validation requires looking up values in your database it is recommended to keep that code in your business
logic. But if the validation is just on the provided data with no lookup required (sometimes referred to as discrete
validations), then a popular library called FluentValidations is very popular to handle those for us.

Here is an example of adding a manual validation to a post request to check if a value already exists.   
First we will update our repository with a function to check if a value supplied matches one that already exists in the
database -

```C#
public async Task<AddressBook> GetAsync(string address)
{
    return await _db.AddressBooks.FirstOrDefaultAsync(u => u.Address.ToLower() == address.ToLower());
}
```

This method takes a string as a parameter and then checks it against each entry in the database. ToLower is called on
both values to ensure that capitalization is ignored.

Now within our endpoint method logic we can call that method supplying the entered value, and if it exists return a 
400 error along with a message confirming the details of the error -

```C#
if (await _repo.GetAsync(addressBookCreateDTO.Address) != null)
{
    return Results.BadRequest("Address already exists");
}
```

### FluentValidations

FluentValidations is a popular nuget package that can handle discrete validations for us.

First install the package -

* FluentValidation.AspNetCore

Next we will add a new folder to the project called Validation, and add a class file with a name for the validations
(e.g. AddressBookCreateValidation)

Within this class we will inherit from the AbstractValidator class and specify the model to be validated, and add
a constructor -

```C#
using FluentValidation;
using MinimalAPI_test.Models.DTO;

namespace MinimalAPI_test.Validation;

public class AddressBookCreateValidation : AbstractValidator<AddressBookCreateDTO>
{
    public AddressBookCreateValidation()
    {
        
    }
}
```

Then add one of the packages built in validators into the constructor -

```C#
RuleFor(model => model.Address).NotEmpty();
```

A list of the built in validators and how to use them can be found on the packages website -
[FluentValidations guide](https://docs.fluentvalidation.net/en/latest/built-in-validators.html)

In the program.cs file add the validator to the services -

```C#
builder.Services.AddValidatorsFromAssemblyContaining<Program>();
```

In your endpoint method logic pass in an instance of IValidator with the type to be validated, then create a variable 
to hold the result of calling the validation and passing in the user object. Then just add an if statement to return
the results (using the linq method FirstOrDefault will return the appropriate error from your validations if there
is more than one) -

```C#
private static async Task<IResult> CreateAddressBook(IAddressBookRepository _repo,
    AddressBookCreateDTO addressBookCreateDTO, ILogger<Program> _logger, IMapper _mapper,
    IValidator<AddressBookCreateDTO> _validation)
{
    var validationResult = await _validation.ValidateAsync(addressBookCreateDTO);
    if (!validationResult.IsValid)
    {
        return Results.BadRequest(validationResult.Errors.FirstOrDefault().ToString());
    }

    if (await _repo.GetAsync(addressBookCreateDTO.Address) != null)
    {
        return Results.BadRequest("Address already exists");
    }

    _logger.Log(LogLevel.Information, "Creating an address book");

    var addressBook = _mapper.Map<AddressBook>(addressBookCreateDTO);
    await _repo.CreateAsync(addressBook);
    await _repo.SaveAsync();

    return Results.Created($"/api/addressbook/{addressBook.Id}", addressBook);
}
```

Note:
: This validation should be carried out before your manual validations if there are any.