# Tutorial - Add Endpoints

First create a clean project by removing the unneeded code from the template app.

[Create a clean project](Create-a-Clean-Project.md)

### Get All Endpoint

```C#
app.MapGet("/api/coupon", () =>
{
    return Results.Ok(CouponStore.couponList);
});
```

### Get Individual Endpoint

```C#
app.MapGet("/api/coupon/{id:int}", (int id) =>
{
    return Results.Ok(CouponStore.couponList.FirstOrDefault(u => u.Id == id));
});
```

### Post wIth 200 Response

```C#
app.MapPost("/api/coupon", ([FromBody] Coupon coupon) =>
{
    if (coupon.Id != 0 || string.IsNullOrEmpty(coupon.Name))
    {
        return Results.BadRequest("Invalid Id or Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exosts");
    }

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    return Results.Ok(coupon);
});
```

### Post with 201 Response

Using the Results.Created method will return a 201 HTTP response (new resource has been created succesfully),
and the URI of the created resource is also returned with the response -

```C#
app.MapPost("/api/coupon", ([FromBody] Coupon coupon) =>
{
    if (coupon.Id != 0 || string.IsNullOrEmpty(coupon.Name))
    {
        return Results.BadRequest("Invalid Id or Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    return Results.Created($"/api/coupon/{coupon.Id}", coupon);
});
```

### Named Endpoints

You can name endpoints with the WithName method at the end of the method and pass in the name -

```C#
app.MapGet("/api/coupon", () =>
{
    return Results.Ok(CouponStore.couponList);
}).WithName("GetCoupons");
```

Using named endpoint in the post request -

```C#
app.MapPost("/api/coupon", ([FromBody] Coupon coupon) =>
{
    if (coupon.Id != 0 || string.IsNullOrEmpty(coupon.Name))
    {
        return Results.BadRequest("Invalid Id or Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id} , coupon);
}).WithName("CreateCoupon");
```

Here rather than Created, CreatedAt Route is used, and the routeName, routeValues and the content is passed in.

### Produces and Accepts

You can tell the API what responses could be returned by the endpoint (otherwise in swagger etc. it will just show
returns 200).   
To do this use the Produces method and pass in the return status code. These should be added to the end of the method
and can be chained -

```C#
app.MapPost("/api/coupon", ([FromBody] Coupon coupon) =>
{
    if (coupon.Id != 0 || string.IsNullOrEmpty(coupon.Name))
    {
        return Results.BadRequest("Invalid Id or Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id} , coupon);
}).WithName("CreateCoupon").Produces<Coupon>(201).Produces(400);
```

In a similar way you can inform the API what data the endpoint accepts using the Accepts method -

```C#
app.MapPost("/api/coupon", ([FromBody] Coupon coupon) =>
{
    if (coupon.Id != 0 || string.IsNullOrEmpty(coupon.Name))
    {
        return Results.BadRequest("Invalid Id or Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id} , coupon);
}).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<Coupon>(201).Produces(400);
```

### ILogger

ILogger can be passed into an endpoint to enable logging. Specify the type as the class we are in (in this case
Program) and name it _logger.   
Within the endpoint method call the _logger and use it to print a message to the consle when the endpoint is run -

```C#
app.MapGet("/api/coupon", (ILogger<Program> _logger) =>
{
    _logger.Log(LogLevel.Information, "Getting all Coupons");
    return Results.Ok(CouponStore.couponList);
}).WithName("GetCoupons").Produces<IEnumerable<Coupon>>(200);
```

### DTOs

You can use Data Transfer Objects to improve the post request so that it does not show id in the required fields.

Create a folder called DTO and create a class with the name for the DTO.

This can then be passed into the post request. A new coupon object will need to be created in the method so that it is
aware of the id field -

```C#
app.MapPost("/api/coupon", ([FromBody] CouponCreateDTO coupon_C_DTO) =>
{
    if (string.IsNullOrEmpty(coupon_C_DTO.Name))
    {
        return Results.BadRequest("Invalid Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon_C_DTO.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    Coupon coupon = new()
    {
        IsActive = coupon_C_DTO.IsActive,
        Name = coupon_C_DTO.Name,
        Percent = coupon_C_DTO.Percent
    };

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id} , coupon);
}).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<Coupon>(201).Produces(400);
```

Another DTO could also be created for the return repsonse to remove certain fields from it -

```C#
app.MapPost("/api/coupon", ([FromBody] CouponCreateDTO coupon_C_DTO) =>
{
    if (string.IsNullOrEmpty(coupon_C_DTO.Name))
    {
        return Results.BadRequest("Invalid Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon_C_DTO.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    Coupon coupon = new()
    {
        IsActive = coupon_C_DTO.IsActive,
        Name = coupon_C_DTO.Name,
        Percent = coupon_C_DTO.Percent
    };

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    CouponDTO couponDTO = new()
    {
        Id = coupon.Id,
        IsActive = coupon.IsActive,
        Name = coupon.Name,
        Percent = coupon.Percent,
        Created = coupon.Created
    };
    
    return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id} , couponDTO);
}).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<Coupon>(201).Produces(400);
```

### AutoMapper

We can remove the need to map to the objects like we did in the previous example by using AutoMapper.

AutoMapper is a separate library that first must be installed.

The config for our mapping can be placed within the program.cs file, but it is a good idea to put it in a 
separate file to keep the application tidy.   
Create a new file within the project called MappingConfig.cs, inherit from the profile class to be able to use its
mapping functions -

```C#
namespace MagicVilla_CouponAPI;

public class MappingConfig : Profile
{

}
```

Add a constructor and within it create the maps by first calling CreateMap with the types to be converted -

```C#
public class MappingConfig : Profile
{
    public MappingConfig()
    {
        CreateMap<Coupon, CouponCreateDTO>();
    }
}
```

For the mapping to work in both directions, call the ReverseMap method -

```C#
public class MappingConfig : Profile
{
    public MappingConfig()
    {
        CreateMap<Coupon, CouponCreateDTO>().ReverseMap();
    }
}
```

Add the remaining required maps -

```C#
public MappingConfig()
{
    CreateMap<Coupon, CouponCreateDTO>().ReverseMap();
    CreateMap<Coupon, CouponDTO>().ReverseMap();
}
```

Now within the program.cs file we can use dependency injection to add our mapping config.

Add the AddAutoMapper service and pass in our config file -

```C#
builder.Services.AddAutoMapper(typeof(MappingConfig));
```

Now pass in the mapper into our post function -

```C#
app.MapPost("/api/coupon", (IMapper _mapper, [FromBody] CouponCreateDTO coupon_C_DTO) =>
```

Now instead of assigning the fields individualy, we can call the Map functionon _mapper, specify the type
we want it assigned to and then pass in the object to be mapped -

```C#
Coupon coupon = _mapper.Map<Coupon>(coupon_C_DTO);
```

So our updated post endpoint will look like this -

```C#
app.MapPost("/api/coupon", (IMapper _mapper, [FromBody] CouponCreateDTO coupon_C_DTO) =>
{
    if (string.IsNullOrEmpty(coupon_C_DTO.Name))
    {
        return Results.BadRequest("Invalid Coupon Name");
    }

    if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon_C_DTO.Name.ToLower()) != null)
    {
        return Results.BadRequest("Coupon Name Already Exists");
    }

    Coupon coupon = _mapper.Map<Coupon>(coupon_C_DTO);

    coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
    CouponStore.couponList.Add(coupon);
    CouponDTO couponDTO = _mapper.Map<CouponDTO>(coupon);
    
    return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id} , couponDTO);
}).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<Coupon>(201).Produces(400);
```

### FluentValidation

Validation rules can be added to your endpoints using the FluentValidation package.

First install the package (FluentValidation.AspNetCore), then again to stop the program.cs file from getting cluttered we will place
the validations in a separate file.   
Create a new folder called Validation, then add a file for the validation (e.g. CouponCreateValidation).

Then inherit from the AbstractValidator class with the type of model to be validated, then add a constructor
and add the required rules -

```C#
using FluentValidation;
using MagicVilla_CouponAPI.Models.DTO;

namespace MagicVilla_CouponAPI.Validation;

public class CouponCreateValidation : AbstractValidator<CouponCreateDTO>
{
    public CouponCreateValidation()
    {
        RuleFor(model => model.Name).NotEmpty();
        RuleFor(model => model.Percent).InclusiveBetween(1, 100);
    }
}
```

In the program.cs file add the validator to the services -

```C#
builder.Services.AddValidatorsFromAssemblyContaining<Program>();
```

Then in your endpoint, first add async then add IValidator with the appropriate type and call it _validation -

```C#
app.MapPost("/api/coupon", async 
    (IMapper _mapper, IValidator<CouponCreateDTO> _validation, [FromBody] CouponCreateDTO coupon_C_DTO) =>
```

Then create a varaiable called validationResult and assign it to await the result of the validation, then perform
a check to see if the state is valid and if it is not return the error -

```C#
var validationResult = await _validation.ValidateAsync(coupon_C_DTO);
if (!validationResult.IsValid)
{
    return Results.BadRequest(validationResult.Errors.FirstOrDefault().ToString());
}
```

So the updated endpoint will now look like this -

```C#
app.MapPost("/api/coupon", async 
    (IMapper _mapper, IValidator<CouponCreateDTO> _validation, [FromBody] CouponCreateDTO coupon_C_DTO) =>
    {
        var validationResult = await _validation.ValidateAsync(coupon_C_DTO);
        if (!validationResult.IsValid)
        {
            return Results.BadRequest(validationResult.Errors.FirstOrDefault().ToString());
        }

        if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon_C_DTO.Name.ToLower()) != null)
        {
            return Results.BadRequest("Coupon Name Already Exists");
        }

        Coupon coupon = _mapper.Map<Coupon>(coupon_C_DTO);

        coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
        CouponStore.couponList.Add(coupon);
        CouponDTO couponDTO = _mapper.Map<CouponDTO>(coupon);

        return Results.CreatedAtRoute("GetCoupon", new { id = coupon.Id }, couponDTO);
    }).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<Coupon>(201).Produces(400);
```

### Create a standard API Response

You can create an API response object to return s standardised result every time. For example in out repose object
rather than just return the data we may wish to return whether it was successful, the status code and any error messages
etc. so the response body would look something like this -

```json
{
  "isSuccess": true,
  "result": [
    {
      "id": 1,
      "name": "10OFF",
      "percent": 10,
      "isActive": true,
      "created": null,
      "lastUpdated": null
    },
    {
      "id": 2,
      "name": " 20OFF",
      "percent": 20,
      "isActive": true,
      "created": null,
      "lastUpdated": null
    }
  ],
  "statusCode": 200,
  "errorMessages": []
}
```

To do this first create a new model called APIResponse and add the required properties -

```C#
using System.Net;

namespace MagicVilla_CouponAPI.Models;

public class APIResponse
{
    public APIResponse()
    {
        ErrorMessages = new List<string>();
    }
    
    public bool isSuccess { get; set; }
    public Object Result { get; set; }
    public HttpStatusCode StatusCode { get; set; }
    public List<string> ErrorMessages { get; set; }
}
```

Then in your endpoints, build up a new APIResponse object and return the results -

```C#
APIResponse response = new();
response.Result = CouponStore.couponList;
response.isSuccess = true;
response.StatusCode = HttpStatusCode.OK;
return Results.Ok(response);
```

SO our updated endpoints will look like this -

```C#
app.MapGet("/api/coupon", (ILogger<Program> _logger) =>
{
    _logger.Log(LogLevel.Information, "Getting all Coupons");
    APIResponse response = new();
    response.Result = CouponStore.couponList;
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}).WithName("GetCoupons").Produces<APIResponse>(200);

app.MapGet("/api/coupon/{id:int}", (ILogger<Program> _logger, int id) =>
{
    APIResponse response = new();
    response.Result = CouponStore.couponList.FirstOrDefault(u => u.Id == id); 
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response); 
}).WithName("GetCoupon").Produces<APIResponse>(200);


app.MapPost("/api/coupon", async 
    (IMapper _mapper, IValidator<CouponCreateDTO> _validation, [FromBody] CouponCreateDTO coupon_C_DTO) =>
    {
        APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest};
         
        
        var validationResult = await _validation.ValidateAsync(coupon_C_DTO);
        if (!validationResult.IsValid)
        {
            response.ErrorMessages.Add(validationResult.Errors.FirstOrDefault().ToString());
            return Results.BadRequest(response);
        }

        if (CouponStore.couponList.FirstOrDefault(u => u.Name.ToLower() == coupon_C_DTO.Name.ToLower()) != null)
        {
            response.ErrorMessages.Add("Coupon Name Already Exists");
            return Results.BadRequest(response);
        }

        Coupon coupon = _mapper.Map<Coupon>(coupon_C_DTO);

        coupon.Id = CouponStore.couponList.OrderByDescending(u => u.Id).FirstOrDefault().Id + 1;
        CouponStore.couponList.Add(coupon);
        CouponDTO couponDTO = _mapper.Map<CouponDTO>(coupon);

        response.Result = couponDTO;
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.Created;
        return Results.Ok(response);
        
    }).WithName("CreateCoupon").Accepts<Coupon>("application/json").Produces<APIResponse>(201).Produces(400);
```