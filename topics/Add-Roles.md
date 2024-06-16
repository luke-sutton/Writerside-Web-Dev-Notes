# Add Roles

Add a policy to the AddAuthorization service within program.cs -

```C#
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AdminOnly", policy => policy.RequireRole("admin"));
});
```

Or if you are using the Authorize attribute on the logic method, add it there -

```C#
[Authorize(Roles = "AdminOnly")]
```

This sets up a policy called AdminOnly that requires the role of admin.

To apply this policy, in your endpoint pass the policy name into the RequireAuthorization method -

```C#
app.MapGet("/api/coupon", GetAllCoupon)
    .WithName("GetCoupons").Produces<APIResponse>(200)
    .RequireAuthorization("AdminOnly");
```

In the repository, as long as the user was given the role of admin they will be able to access the endpoint -

```C#
public async Task<UserDTO> Register(RegistrationRequestDTO requestDto)
{
    LocalUser userObj = new()
    {
        UserName = requestDto.UserName,
        Password = requestDto.Password,
        Name = requestDto.Name,
        Role = "admin"
    };
    
    _db.LocalUsers.Add(userObj);
    await _db.SaveChangesAsync();
    userObj.Password = "";
    return _mapper.Map<UserDTO>(userObj);
}
```

But if you set a different role such as customer, they will not have access to the endpoint.