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
private static async Task<ICollection<AddressBook>> GetAllAddressBook(IAddressBookRepository _repo)
{
    ICollection<AddressBook> addressBookEntities = await _repo.GetAllAsync();
    return addressBookEntities;
}
```

We should now be able to run the application and successfully call the endpoint.