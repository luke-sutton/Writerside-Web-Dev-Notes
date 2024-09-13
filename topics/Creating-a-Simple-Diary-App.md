# Creating the Database

These are the steps required to set up and connect to the database, and create the required table etc

## Create the Model for the Database

First, we need a model to represent the data that will be held in our database.

For the diary app we are going to create we need a database that has the following columns:

Id   
Title   
Content   
Date

Right-click on the Models folder in the solution explorer, select add, then select class. Enter the name DiaryEntry.

```C#
namespace lambdaluke_mvc_example.Models;

public class DiaryEntry
{
    
}
```

Add the properties we need for the database:

```C#
namespace lambdaluke_mvc_example.Models;

public class DiaryEntry
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime Created { get; set; }
}
```

We will use EntityFramework to create our database using this model. The name given to the table will be the name of
the model, DiaryEntry. And the columns will be named after our four properties.

Note:
: The name of the property / column that will be used as the identifier needs to be called either Id, or the name of the
class followed by Id (DiaryEntryId). If you wish to use another name, then it requires a key attribute like this:

```C#
[Key]
public int Identifier { get; set; }
```

But if the name Id, or DiaryEntryId is used, then this is not required.

To prevent the user from creating entries in the database with null values, add the required attribute to the
properties. And to prevent any possible null reference exceptions, initialize the values so they can never be empty:

```C#
public int Id { get; set; }
[Required]s
public string Title { get; set; } = string.Empty;
[Required]
public string Content { get; set; } = string.Empty;
[Required]
public DateTime Created { get; set; } = DateTime.Now;
```

## Run SQL Server

First, ensure that SQL Server is up and running in a docker container.   
Use the following guide to set this up if required: [Install MS SQL Server On Mac](Install-MS-SQL-Server-On-Mac.md)

Open DataGrip (either app or plugin) select connect to database, then select add data source manually.   
Set data source to Microsoft SQL Server   
Enter port number as 1433
Enter the server name as localhost   
Enter the user name as appropriate   
Enter the password as appropriate   
Click test connection   
If test was successful click connect to database


## Add a Connection String

Next, we need to add a connection string to link our application to the database.

Update the appsettings.json file with a connection string:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost; Database=DiaryTest; Trusted_Connection=false; TrustServerCertificate=True; User Id=sa; Password=MyPassword;"
  }
}
```

For the database field, you should enter whatever you would like the created database to be called. For user id and
password, you should use the appropriate details for your SQL Server instance.

## Install the Required Packages

Next, we need to install EntityFrameworkCore in our application.   
Go to the Nuget Package manager, and search for and add the following packages to the project:

- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

Note:
: If you go the csproj file, you will now see the references to the added packages.

## Create a Database Context Class

next, we need to create a database context class called `ApplicationDbContext`, which is a part of Entity Framework
Core. This context class is used to interact with the database.

Create a new folder within the project called Data. Then add a new class file called ApplicationDbContext:

```C#
namespace lambdaluke_mvc_example.Data;

public class ApplicationDbContext
{
    
}
```

Add a constructor and inherit from `DbContext`, which is a class from EF Core that manages database connections and is
used to query or save data to the database. Pass in a parameter called `options` of type
`DbContextOptions<ApplicationDbContext>`.`DbContextOptions` encapsulates all the configuration related to `DbContext`,
such as database connection strings, database provider (SQL Server, SQLite, etc.), and other database-specific settings.
Pass this to the base class using `: base(options)`. This ensures that the DbContext is properly configured with those
options.

```C#
using Microsoft.EntityFrameworkCore;

namespace lambdaluke_mvc_example.Data;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
        
    }
}
```

Add a DbSet property of type DiaryEntries (the model we created for our table)

```C#
using lambdaluke_mvc_example.Models;
using Microsoft.EntityFrameworkCore;

namespace lambdaluke_mvc_example.Data;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
        
    }
    
    public DbSet<DiaryEntry> DiaryEntries { get; set; }
}
```

DbSet is a property declaration within the ApplicationDbContext class, which is part of an Entity Framework Core 
context. This property maps the DiaryEntry model class to a table in the database.

## Add a Service

Next, we need to register our database context class as a service by adding the following line to the Program.cs file:

```C#
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

This line registers our class as a service, then configures it to use SQL Server as the database provider, then
retrieves our connection string.

This registers ApplicationDbContext as a service in the application's dependency injection container.
This setup enables the application to inject ApplicationDbContext wherever needed and ensures it is properly configured
to interact with the specified SQL Server database.

## Add a Migration

A migration is an instruction to how we want to modify our database.

Right click on the project name in the solution explorer, select Entity Framework Core then select add migration.

The add migration window will open, with a default name for the migration of 'Initial'. You can either keep this name
or change it to something like the name for our table.   
Press ok.

Once the process has finished a Migrations folder will be added to the project. This folder contains the class files
with the instruction to build our table.

Now, to run the migration to actually create the database and table, right-click on the solution name, select Entity
Framework Core, then select update database, then press ok on the window that opens.

To view our table in the database open DataGrip, right-click on the connection, select tools then select manage
shown schemas. Select the name of the database you created (it will have been given the name you set in your
connection string). Then you can expand the options within the database until you see tables, and the name of
our new table (DiaryEntries).

Double-clicking on the table name will show the table columns.

Note:
: If the database does not appear in the list of available schemas, try refreshing the connection, or select add
all schemas.