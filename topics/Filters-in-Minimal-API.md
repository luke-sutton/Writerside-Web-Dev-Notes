# Filters in Minimal API

Note:
: This was introduced in .Net 7

Below is an example of adding a filter to perform validation on a get single endpoint -

```C#
app.MapGet("/api/coupon/{id:int}", GetCoupon).WithName("GetCoupon")
    .Produces<APIResponse>(200)
    .AddEndpointFilter(async (context, next) =>
    {
        var id = context.GetArgument<int>(2);
        if (id == 0)
        {
            return Results.BadRequest("Cannot have 0 as id");
        }
    
        return await next(context);
    });
```

In this line -

```C#
var id = context.GetArgument<int>(2);
```

GetArgument is of type int and has the number 2 passed in, this is because the parameter we are checking is the 3rd
parameter in the method and is of type int -

```C#
private async static Task<IResult> GetCoupon(ICouponRepository _couponRepo, ILogger<Program> _logger, int id)
{
    APIResponse response = new();
    response.Result = await _couponRepo.GetAsync(id);
    response.isSuccess = true;
    response.StatusCode = HttpStatusCode.OK;
    return Results.Ok(response);
}
```

### Chaining Filters

It is possible to chain filters in the following way -

```C#
app.MapGet("/api/coupon/{id:int}", GetCoupon).WithName("GetCoupon")
    .Produces<APIResponse>(200)
    .AddEndpointFilter(async (context, next) =>
    {
        var id = context.GetArgument<int>(2);
        if (id == 0)
        {
            return Results.BadRequest("Cannot have 0 as id");
        }

        return await next(context);
    }).AddEndpointFilter(async (context, next) =>
    {
        Console.WriteLine("2nd filter");

        return await next(context);
    });
```