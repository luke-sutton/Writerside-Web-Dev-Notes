# Add a Modal

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