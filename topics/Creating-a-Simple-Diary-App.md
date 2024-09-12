# Creating a Simple Diary App

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

## Creating the Database

First, ensure that SQL Server is up and running.   
Use the following guide to set this up if required: [Install MS SQL Server On Mac](Install-MS-SQL-Server-On-Mac.md)

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

Next, we need to install EntityFrameworkCore in our application.   
Go to the Nuget Package manager, and search for and add the following packages to the project:

- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

Note:
: If you go the csproj file, you will now see the references to the added packages.