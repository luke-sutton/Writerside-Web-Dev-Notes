# Tutorial - Database and Repository

### Install Necessary Libraries

* Microsoft.EntityFrameworkCore.SqlServer
* Microsoft.EntityFrameworkCore.Tools

### Add Connection String

Open the appsettings.json file and add a new object called ConnectionStrings, then a key to use to name the connection (e.g. DefaultConnection),
Then enter the details as the value -

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost; Database=MagicVilla_CouponAPI; Trusted_Connection=false; TrustServerCertificate=True; User Id=sa; Password=password1;"
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
: The name you give the database here will be the one created by entity framework.

### Add DbContext DAL

Add a ApplicationDbContext file to the Data folder -

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
public DbSet<Coupon> Coupons { get; set; }
```

Next add a method called onModelCreating and pass in the objects to be used in the database -

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Coupon>().HasData(
            new Coupon { Id = 1, Name = "10OFF", Percent = 10, IsActive = true },
            new Coupon { Id = 2, Name = "20OFF", Percent = 20, IsActive = true }
        );
    }
```

Complete DAL -

```C#
using MagicVilla_CouponAPI.Models;
using Microsoft.EntityFrameworkCore;

namespace MagicVilla_CouponAPI.Data;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
        
    }

    public DbSet<Coupon> Coupons { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Coupon>().HasData(
            new Coupon { Id = 1, Name = "10OFF", Percent = 10, IsActive = true },
            new Coupon { Id = 2, Name = "20OFF", Percent = 20, IsActive = true }
        );
    }

```

Add the DbContext to program.cs -

```C#
builder.Services.AddDbContext<ApplicationDbContext>(option =>
    option.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

Note:
: Ensure Microsoft Sql Server is up and running.

We can now use Entity Framework to create our database and table

Note:
: Following notes are specific to Rider IDE

Right-click on the projects name, there will be a section titled Entity Framework Core, within this menu select Add
Migration.   
In the window that opens give an appropriate name for the migration (e.g. AddCouponToDb) and press ok.   
A New folder titled Migrations will be added to the project containing the migration file.   
Right-click on the project again and go to the Entity Framework Core menu and this time select update database.   
The database and table will be auto created and the data in your DAL OnModelCreating method will be seeded to the
table.

Note:
: If you receive an error regarding Entity Framework tools being out of date, update them by running the following 
command in the console -
```Bash
 dotnet tool update --global dotnet-ef
```

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

### Update Endpoints

We can now delete the store we were using and instead use our database.

We have already added the ApplicationDbContext to the services, and now need to pass it into the endpoints -

```C#
app.MapGet("/api/coupon", async (ApplicationDbContext _db, ILogger<Program> _logger) =>
```

And instead of using CouponStore.couponlist we can now use _db.Coupons -

```C#
response.Result = _db.Coupons;
```

When performing a search or an action in the database we should use async methods and await the result.   
E.g. instead of using FirstOrDefault we can use FirstOrDefaultAsync -

```C#
response.Result = await _db.Coupons.FirstOrDefaultAsync(u => u.Id == id);
```

When creating a new entry or updaing an entry we need to save the changes with SaveChangesAsync -

```C#
_db.Coupons.Add(coupon);
    await _db.SaveChangesAsync();
```

Here is the completed updated endpoints -

```C#
app.MapGet("/api/coupon", async (ApplicationDbContext _db, ILogger<Program> _logger) =>
{
    _logger.Log(LogLevel.Information, "Getting all Coupons");
    APIResponse response = new();
    response.Result = _db.Coupons;
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("GetCoupons").Produces<APIResponse>(200);

app.MapGet("/api/coupon/{id:int}", async (ApplicationDbContext _db, ILogger<Program> _logger, int id) =>
{
    APIResponse response = new();
    response.Result = await _db.Coupons.FirstOrDefaultAsync(u => u.Id == id);
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("GetCoupon").Produces<APIResponse>(200);


app.MapPost("/api/coupon", async
    (ApplicationDbContext _db, IMapper _mapper, IValidator<CouponCreateDTO> _validation, [FromBody] CouponCreateDTO coupon_C_DTO) =>
{
    APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

    var validationResult = await _validation.ValidateAsync(coupon_C_DTO);
    if (!validationResult.IsValid)
    {
        response.ErrorMessages.Add(validationResult.Errors.FirstOrDefault().ToString());
        return Results.BadRequest(response);
    }

    if (await _db.Coupons.FirstOrDefaultAsync(u => u.Name.ToLower() == coupon_C_DTO.Name.ToLower()) != null)
    {
        response.ErrorMessages.Add("Coupon Name Already Exists");
        return Results.BadRequest(response);
    }

    Coupon coupon = _mapper.Map<Coupon>(coupon_C_DTO);
    
    _db.Coupons.Add(coupon);
    await _db.SaveChangesAsync();
    CouponDTO couponDTO = _mapper.Map<CouponDTO>(coupon);

    response.Result = couponDTO;
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.Created;
    return Results.Ok(response);
}).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<APIResponse>(201).Produces(400);


app.MapPut("/api/coupon", async
    (ApplicationDbContext _db, IMapper _mapper, IValidator<CouponUpdateDTO> _validation, [FromBody] CouponUpdateDTO coupon_U_DTO) =>
{
    APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

    var validationResult = await _validation.ValidateAsync(coupon_U_DTO);
    if (!validationResult.IsValid)
    {
        response.ErrorMessages.Add(validationResult.Errors.FirstOrDefault().ToString());
        return Results.BadRequest(response);
    }

    Coupon couponFromStore = await _db.Coupons.FirstOrDefaultAsync(u => u.Id == coupon_U_DTO.Id);
    couponFromStore.IsActive = coupon_U_DTO.IsActive;
    couponFromStore.Name = coupon_U_DTO.Name;
    couponFromStore.Percent = coupon_U_DTO.Percent;
    couponFromStore.LastUpdated = DateTime.Now;

    await _db.SaveChangesAsync();
    
    response.Result = _mapper.Map<Coupon>(couponFromStore);
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("UpdateCoupon").Accepts<CouponUpdateDTO>("application/json").Produces<APIResponse>(200).Produces(400);


app.MapDelete("/api/coupon/{id:int}", async (ApplicationDbContext _db, int id) =>
{
    APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

    Coupon couponFromStore = await _db.Coupons.FirstOrDefaultAsync(u => u.Id == id);

    if (couponFromStore != null)
    {
        _db.Coupons.Remove(couponFromStore);
        await _db.SaveChangesAsync();
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.NoContent;
        return Results.Ok(response);
    }
    else
    {
        response.ErrorMessages.Add("Invalid Id");
        return Results.BadRequest(response);
    }
});
```

### Add a Repository

Create a Folder called Repository, and add an interface called ICouponRepository -

```C#
using MagicVilla_CouponAPI.Models;

namespace MagicVilla_CouponAPI.Repository;

public interface ICouponRepository
{
    Task<ICollection<Coupon>> GetAllAsync();
    Task<Coupon> GetAsync(int id);
    Task<Coupon> GetAsync(string couponName);
    Task CreateAsync(Coupon coupon);
    Task UpdateAsync(Coupon coupon);
    Task RemoveAsync(Coupon coupon);
    Task SaveAsync();
}
```

Create a new class in the folder called CouponRepository that inherits from the interface and implements the methods -

```C#
using MagicVilla_CouponAPI.Data;
using MagicVilla_CouponAPI.Models;
using Microsoft.EntityFrameworkCore;

namespace MagicVilla_CouponAPI.Repository;

public class CouponRepository : ICouponRepository
{
    private readonly ApplicationDbContext _db;
    
    public CouponRepository(ApplicationDbContext db)
    {
        _db = db;
    }
    
    public async Task<ICollection<Coupon>> GetAllAsync()
    {
       return await _db.Coupons.ToListAsync();
    }

    public async Task<Coupon> GetAsync(int id)
    {
        return await _db.Coupons.FirstOrDefaultAsync(u => u.Id == id);
    }

    public async Task<Coupon> GetAsync(string couponName)
    {
        return await _db.Coupons.FirstOrDefaultAsync(u => u.Name.ToLower() == couponName.ToLower());
    }

    public async Task CreateAsync(Coupon coupon)
    {
        _db.Add(coupon);
    }

    public async Task UpdateAsync(Coupon coupon)
    {
        _db.Coupons.Update(coupon);
    }

    public async Task RemoveAsync(Coupon coupon)
    {
        _db.Coupons.Remove(coupon);
    }

    public async Task SaveAsync()
    {
        await _db.SaveChangesAsync();
    }
}
```

### Modify Endpoints to Use Repository

Add the interface and its implementation to the services -

```C#
builder.Services.AddScoped<ICouponRepository, CouponRepository>();
```

In the endpoints, instead of passing in the database context, pass in the coupon repository -

```C#
app.MapGet("/api/coupon", async (ICouponRepository _couponRepo, ILogger<Program> _logger) =>
```

Now instead of called methods on _db, you can call the methods in the interface -

```C#
Coupon couponFromStore = await _couponRepo.GetAsync(id);
```

Here are the updated endpoints -

```C#
app.MapGet("/api/coupon", async (ICouponRepository _couponRepo, ILogger<Program> _logger) =>
{
    _logger.Log(LogLevel.Information, "Getting all Coupons");
    APIResponse response = new();
    response.Result = await _couponRepo.GetAllAsync();
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("GetCoupons").Produces<APIResponse>(200);

app.MapGet("/api/coupon/{id:int}", async (ICouponRepository _couponRepo, ILogger<Program> _logger, int id) =>
{
    APIResponse response = new();
    response.Result = await _couponRepo.GetAsync(id);
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("GetCoupon").Produces<APIResponse>(200);


app.MapPost("/api/coupon", async
    (ICouponRepository _couponRepo, IMapper _mapper, IValidator<CouponCreateDTO> _validation, [FromBody] CouponCreateDTO coupon_C_DTO) =>
{
    APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

    var validationResult = await _validation.ValidateAsync(coupon_C_DTO);
    if (!validationResult.IsValid)
    {
        response.ErrorMessages.Add(validationResult.Errors.FirstOrDefault().ToString());
        return Results.BadRequest(response);
    }

    if (await _couponRepo.GetAsync(coupon_C_DTO.Name) != null)
    {
        response.ErrorMessages.Add("Coupon Name Already Exists");
        return Results.BadRequest(response);
    }

    Coupon coupon = _mapper.Map<Coupon>(coupon_C_DTO);

    await _couponRepo.CreateAsync(coupon);
    await _couponRepo.SaveAsync();
    CouponDTO couponDTO = _mapper.Map<CouponDTO>(coupon);

    response.Result = couponDTO;
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.Created;
    return Results.Ok(response);
}).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<APIResponse>(201).Produces(400);


app.MapPut("/api/coupon", async
    (ICouponRepository _couponRepo, IMapper _mapper, IValidator<CouponUpdateDTO> _validation, [FromBody] CouponUpdateDTO coupon_U_DTO) =>
{
    APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

    var validationResult = await _validation.ValidateAsync(coupon_U_DTO);
    if (!validationResult.IsValid)
    {
        response.ErrorMessages.Add(validationResult.Errors.FirstOrDefault().ToString());
        return Results.BadRequest(response);
    }
    
    await _couponRepo.UpdateAsync(_mapper.Map<Coupon>(coupon_U_DTO));
    await _couponRepo.SaveAsync();
    
    response.Result = _mapper.Map<Coupon>(await _couponRepo.GetAsync(coupon_U_DTO.Id));
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("UpdateCoupon").Accepts<CouponUpdateDTO>("application/json").Produces<APIResponse>(200).Produces(400);


app.MapDelete("/api/coupon/{id:int}", async (ICouponRepository _couponRepo, int id) =>
{
    APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

    Coupon couponFromStore = await _couponRepo.GetAsync(id);

    if (couponFromStore != null)
    {
        await _couponRepo.RemoveAsync((couponFromStore));
        await _couponRepo.SaveAsync();
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.NoContent;
        return Results.Ok(response);
    }
    else
    {
        response.ErrorMessages.Add("Invalid Id");
        return Results.BadRequest(response);
    }
});
```