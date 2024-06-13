# Tutorial - Move Endpoints Into Seperate Class

Create a new folder in the project called Endpoints, then add a new class called CouponEndpoints.

Make the class static then add a static method called ConfigureCouponEndpoints, and pass in WebApplication -

```C#
namespace MagicVilla_CouponAPI.Endpoints;

public static class CouponEndpoints
{
    public static void ConfigureCouponEndpoints(this WebApplication app)
    {
    }
}
```

Then just cut the endpoints from Program.cs and add them to the new method, and then add the required using statements -

```C#
using System.Net;
using AutoMapper;
using FluentValidation;
using MagicVilla_CouponAPI.Models;
using MagicVilla_CouponAPI.Models.DTO;
using MagicVilla_CouponAPI.Repository;
using Microsoft.AspNetCore.Mvc;

namespace MagicVilla_CouponAPI.Endpoints;

public static class CouponEndpoints
{
    public static void ConfigureCouponEndpoints(this WebApplication app)
    {
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
        (ICouponRepository _couponRepo, IMapper _mapper, IValidator<CouponCreateDTO> _validation,
            [FromBody] CouponCreateDTO coupon_C_DTO) =>
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
            (ICouponRepository _couponRepo, IMapper _mapper, IValidator<CouponUpdateDTO> _validation,
                [FromBody] CouponUpdateDTO coupon_U_DTO) =>
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
            }).WithName("UpdateCoupon").Accepts<CouponUpdateDTO>("application/json").Produces<APIResponse>(200)
            .Produces(400);


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
    }
}
```

Then in the Program.cs we need to invoke the new class by calling app.ConfigureCouponEndpoints() -

```C#
using FluentValidation;
using MagicVilla_CouponAPI;
using MagicVilla_CouponAPI.Data;
using MagicVilla_CouponAPI.Endpoints;
using MagicVilla_CouponAPI.Repository;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
builder.Services.AddScoped<ICouponRepository, CouponRepository>();
builder.Services.AddDbContext<ApplicationDbContext>(option =>
    option.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddValidatorsFromAssemblyContaining<Program>();
builder.Services.AddAutoMapper(typeof(MappingConfig));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.ConfigureCouponEndpoints();

app.UseHttpsRedirection();

app.Run();
```

### Clean Up Endpoints Structure

We can clean up the endpoints further by moving the logic into separate functions. Here is how we can do this on the
get all endpoint -

At the bottom of your endpoint class create a new private async static function that returns an IEnumerable of type
IResult and call it GetAllCoupon -

```C#
private async static Task<IResult> GetAllCoupon()
{

}
```

Then cut the parameters from the get all endpoint and pass them to this function then cut the logic and use it here -

```C#
private async static Task<IResult> GetAllCoupon(ICouponRepository _couponRepo, ILogger<Program> _logger)
{
    _logger.Log(LogLevel.Information, "Getting all Coupons");
    APIResponse response = new();
    response.Result = await _couponRepo.GetAllAsync();
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}
```

The endpoint can now be updated to just pass in this function -

```C#
app.MapGet("/api/coupon", GetAllCoupon).WithName("GetCoupons").Produces<APIResponse>(200);
```

Completed endpoints class -

```C#
using System.Net;
using AutoMapper;
using FluentValidation;
using MagicVilla_CouponAPI.Models;
using MagicVilla_CouponAPI.Models.DTO;
using MagicVilla_CouponAPI.Repository;
using Microsoft.AspNetCore.Mvc;

namespace MagicVilla_CouponAPI.Endpoints;

public static class CouponEndpoints
{
    public static void ConfigureCouponEndpoints(this WebApplication app)
    {
        app.MapGet("/api/coupon", GetAllCoupon).WithName("GetCoupons").Produces<APIResponse>(200);

        app.MapGet("/api/coupon/{id:int}", GetCoupon).WithName("GetCoupon").Produces<APIResponse>(200);


        app.MapPost("/api/coupon", CreateCoupon).WithName("CreateCoupon").Accepts<Coupon>("application/json")
            .Produces<APIResponse>(201).Produces(400);


        app.MapPut("/api/coupon", UpdateCoupon).WithName("UpdateCoupon").Accepts<CouponUpdateDTO>("application/json")
            .Produces<APIResponse>(200)
            .Produces(400);


        app.MapDelete("/api/coupon/{id:int}", DeleteCoupon);
    }

    private async static Task<IResult> GetAllCoupon(ICouponRepository _couponRepo, ILogger<Program> _logger)
    {
        _logger.Log(LogLevel.Information, "Getting all Coupons");
        APIResponse response = new();
        response.Result = await _couponRepo.GetAllAsync();
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.OK;
        return Results.Ok(response);
    }

    private async static Task<IResult> GetCoupon(ICouponRepository _couponRepo, ILogger<Program> _logger, int id)
    {
        APIResponse response = new();
        response.Result = await _couponRepo.GetAsync(id);
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.OK;
        return Results.Ok(response);
    }

    private async static Task<IResult> CreateCoupon(ICouponRepository _couponRepo, IMapper _mapper,
        IValidator<CouponCreateDTO> _validation,
        [FromBody] CouponCreateDTO coupon_C_DTO)
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
    }

    private async static Task<IResult> UpdateCoupon(ICouponRepository _couponRepo, IMapper _mapper,
        IValidator<CouponUpdateDTO> _validation,
        [FromBody] CouponUpdateDTO coupon_U_DTO)
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
    }

    private async static Task<IResult> DeleteCoupon(ICouponRepository _couponRepo, int id)
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
    }
}
```