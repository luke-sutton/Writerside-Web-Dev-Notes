# Add a Modal &amp; Data Store

### Models

Create a folder within the project called Models then add a new class file with the models name -

```C#
namespace MagicVilla_CouponAPI.Models;

public class Coupon
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public int Percent { get; set; }
    public bool IsActive { get; set; }
    public DateTime? Created { get; set; }
    public DateTime? LastUpdated { get; set; }
}
```

### Data Store

Normally in a project you will have a database to store the applications data, but to keep things simple we
will store are data in an object in a data store.

Create a folder called Data within the project then add a new class file with the name of your data store -

```C#
using MagicVilla_CouponAPI.Models;

namespace MagicVilla_CouponAPI.Data;

public static class CouponStore
{
    public static List<Coupon> couponList = new List<Coupon>
    {
        new Coupon { Id = 1, Name = "10OFF", Percent = 10, IsActive = true },
        new Coupon { Id = 1, Name = " 20OFF", Percent = 20, IsActive = true }
    };
}
```