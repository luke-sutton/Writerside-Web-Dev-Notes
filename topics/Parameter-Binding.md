# Parameter Binding

Note:
: This was introduced in .Net 7

If you have an endpoint with a lot of parameters, such as in this example here -

```C#
app.MapGet("/api/coupon/special", (string CouponName, int PageSize, int Page, ILogger<Program> _logger, ApplicationDbContext _db) =>
{
    if (CouponName != null)
    {
        return _db.Coupons.Where(u => u.Name.Contains(CouponName)).Skip((Page - 1) * PageSize).Take(PageSize);
    }
    return _db.Coupons.Skip((Page - 1) * PageSize).Take(PageSize);
});
```

You can use a technique called parameter binding to make the code more concise.

First create a modal for the parameters required in the endpoint (aside from the ApplicationDbContext) -

```C#
class CouponRequest
{
    public string CouponName { get; set; }
    [FromHeader(Name = "PageSize")]
    public int PageSize { get; set; }
    [FromHeader(Name = "Page")]
    public int Page { get; set; }
    public ILogger<CouponRequest> Logger { get; set; }
}
```

And then pass this as a parameter into your endpoint, using [AsParameters] before it -

```C#
app.MapGet("/api/coupon/special", ( [AsParameters] CouponRequest req, ApplicationDbContext _db) =>
{
    if (req.CouponName != null)
    {
        return _db.Coupons.Where(u => u.Name.Contains(req.CouponName)).Skip((req.Page - 1) * req.PageSize).Take(req.PageSize);
    }
    return _db.Coupons.Skip((req.Page - 1) * req.PageSize).Take(req.PageSize);
});
```

In the logic you just need to add the object name infront of where the parameteres are used. E.g. in the above example
instead of called PageSize, call req.PageSize.