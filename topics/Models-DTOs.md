# Models &amp; DTOs

### Add a Modal

Create a folder titled Modals within the project and then add a new file with the name of your database table.

Create fields for the columns in the table -

```C#
namespace DotNetAPI.Models;

public partial class User
{
    public int UserId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string Gender { get; set; }
    public bool Active { get; set; }
}
```

A warning will be displayed on the string properties that there value could be null. To correct this
add a constructor and assign their value to an empty string if null -

```C#
public User()
{
    if (FirstName == null)
    {
        FirstName = "";
    }

    if (LastName == null)
    {
        LastName = "";
    }

    if (Email == null)
    {
        Email = "";
    }

    if (Gender == null)
    {
        Gender = "";
    }
}
```

### DTO - Data Transfer Objects

Data Transfer Objects are just models created for the purpose of transferring data.

For example, you may have a model which is a direct representation of a table in your database,
but this table features a primary key column that is auto created, and so we do not want to supply one when
using a put request to add a new entry. And so a direct copy of that model is made but with the primary key
column removed which can then be used in the put request.

DTOs are typically placed in a folder titled Dtos, then you can simply copy the relevant model file into here,
then just rename the file and the class to add Dto and the reason for it to the end (e.g. user could become
userToAddDto), and just remove the unneeded property/s.