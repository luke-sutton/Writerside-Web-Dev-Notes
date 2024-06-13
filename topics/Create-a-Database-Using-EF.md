# Create a Database Using EF

### Install Necessary Libraries

* Microsoft.EntityFrameworkCore.SqlServer
* Microsoft.EntityFrameworkCore.Tools

### Add a Model

Create a folder called Models and add a new class file, the model should be a representation of the columns you need in
your database table -

```C#
namespace MinimalAPI_test.Models;

public class AddressBook
{
    public int Id { get; set; }
    public string? FirstName { get; set; }
    public string? Surname { get; set; }
    public int Age { get; set; }
    public string? Address { get; set; }
}
```

### Add Connection String

Add the connection string to the appsettings.json file -

```C#
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost; Database=DemoDatabase; Trusted_Connection=false; TrustServerCertificate=True; User Id=sa; Password=password1;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

Note:
: The name given to the Database key here will be the name of the database that Entity Framework creates.

### Add Data Access Layer

Create a folder called Data, and add a file called ApplicationDbContext

```C#
using Microsoft.EntityFrameworkCore;

namespace MagicVilla_CouponAPI.Data;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
        
    }
}
```

Add a DbSet property, the name given to this will be the name of the table that is created

```C#
public DbSet<Coupon> AddressBook { get; set; }
```

Next add a method called onModelCreating and pass in the objects to be seeded to the database -

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<AddressBook>().HasData(
        new AddressBook { Id = 1, FirstName = "James", Surname = "Bond", Age = 38, Address = "London"},
        new AddressBook { Id = 2, FirstName = "Sherlock", Surname = "Holmes", Age = 45, Address = "London"}
    );
}
```

Complete DAL -

```C#
using Microsoft.EntityFrameworkCore;
using MinimalAPI_test.Models;

namespace MinimalAPI_test.Data;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {

    }
    
    public DbSet<AddressBook> Coupons { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AddressBook>().HasData(
            new AddressBook { Id = 1, FirstName = "James", Surname = "Bond", Age = 38, Address = "London"},
            new AddressBook { Id = 2, FirstName = "Sherlock", Surname = "Holmes", Age = 45, Address = "London"}
        );
    }
}
```

Add the DbContext to program.cs -

```C#
builder.Services.AddDbContext<ApplicationDbContext>(option =>
    option.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

### Create the Database

Note:
: Ensure Microsoft Sql Server is up and running.

We can now use Entity Framework to create our database and table

Note:
: Following notes are specific to Rider IDE

Right-click on the projects name, there will be a section titled Entity Framework Core, within this menu select Add
Migration.   
In the window that opens give an appropriate name for the migration (e.g. AddAddressBookToDb) and press ok.   
A New folder titled Migrations will be added to the project containing the migration file.

Right-click on the project again and go to the Entity Framework Core menu and this time select update database.   
The database and table will be auto created and the data in your DAL OnModelCreating method will be seeded to the
table.

Note:
: If you receive an error regarding 'Only the invariant culture is supported in globalization-invariant mode', you
can resolve this by editing the csprog file, by selecting the file, right-clicking and selecting the edit menu, then
selecting Edit 'Filename.csprog' (Or on Mac you can just select the project file and press command + down).   
Then set the value of the following line to false -
```C#
<InvariantGlobalization>false</InvariantGlobalization>
```

You can then connect to the database to view the table. In rider open the right hand database window and select new
connection. Select use connection string, then just paste in the value of your connection string and press next.

You should now be able to see your newly created database, table and seeded data.