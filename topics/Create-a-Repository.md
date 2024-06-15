# Create a Repository

Note:
: A repository, also known as a database repository, refers to an abstraction layer between the data in a program,
typically stored in a database, and the business logic of the program.

: The repository layer handles all the work that involves data manipulation. This includes all CRUD operations (Create,
Read, Update, Delete) and other data-related tasks such as querying, sorting, filtering, and transforming of data.

: A repository facilitates a separation of concerns wherein the code involving business logic or presentation is not
concerned with data access details. This makes the code more maintainable, readable, and testable.

Create a folder called Repository, add an interface named after your table (e.g. IAddressBookRepository) -

```C#
namespace MinimalAPI_test.Repository;

public interface IAddressBookRepository
{

}
```

Add tasks that you would like to perform such as get all, get, post etc. -

```C#
using MinimalAPI_test.Models;

namespace MinimalAPI_test.Repository;

public interface IAddressBookRepository
{
    Task<ICollection<AddressBook>> GetAllAsync();
    Task<AddressBook> GetAsync(int id);
    Task<AddressBook> GetAsync(string firstName);
    Task CreateAsync(AddressBook addressBook);
    Task UpdateAsync(AddressBook addressBook);
    Task RemoveAsync(AddressBook addressBook);
    Task SaveAsync();
}
```

Next create a class file for the repository (e.g. AddressBookRepository) and inherit from the interface -

```C#
using Microsoft.EntityFrameworkCore;

namespace MinimalAPI_test.Repository;

public class AddressBookRepository : IAddressBookRepository
{

}
```

Create a property of type ApplicationDbContext called _db, then add a constructor using this -

```C#
using Microsoft.EntityFrameworkCore;
using MinimalAPI_test.Data;
using MinimalAPI_test.Models;

namespace MinimalAPI_test.Repository;

public class AddressBookRepository : IAddressBookRepository
{
    private readonly ApplicationDbContext _db;

    public AddressBookRepository(ApplicationDbContext db)
    {
        _db = db;
    }
}
```

Finally, implement all the methods in the interface -

```C#
using Microsoft.EntityFrameworkCore;
using MinimalAPI_test.Data;
using MinimalAPI_test.Models;

namespace MinimalAPI_test.Repository;

public class AddressBookRepository : IAddressBookRepository
{
    private readonly ApplicationDbContext _db;

    public AddressBookRepository(ApplicationDbContext db)
    {
        _db = db;
    }

    public async Task<ICollection<AddressBook>> GetAllAsync()
    {
        return await _db.AddressBooks.ToListAsync();
    }

    public async Task<AddressBook> GetAsync(int id)
    {
        return await _db.AddressBooks.FirstOrDefaultAsync(u => u.Id == id);
    }

    public async Task<AddressBook> GetAsync(string firstName)
    {
        return await _db.AddressBooks.FirstOrDefaultAsync(u => u.FirstName.ToLower() == firstName.ToLower());
    }

    public async Task CreateAsync(AddressBook addressbook)
    {
        _db.Add(addressbook);
    }

    public async Task UpdateAsync(AddressBook addressbook)
    {
        _db.AddressBooks.Update(addressbook);
    }

    public async Task RemoveAsync(AddressBook addressbook)
    {
        _db.AddressBooks.Remove(addressbook);
    }

    public async Task SaveAsync()
    {
        await _db.SaveChangesAsync();
    }
}
```

Now back in the program.cs file you need to add the repository to the services -

```C#
builder.Services.AddScoped<IAddressBookRepository, AddressBookRepository>();
```