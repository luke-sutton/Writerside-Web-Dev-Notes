# Authentication &amp; Authorization

### Model, DAL & Database - Entity Framework

Add a new Model called LocalUser -

```C#
namespace MagicVilla_CouponAPI.Models;

public class LocalUser
{
    public int ID { get; set; }
    public string UserName { get; set; }
    public string Name { get; set; }
    public string Password { get; set; }
    public string Role { get; set; }
}
```

Update the ApplicationDbContext file (DAL) with a new property for this model -

```C#
public DbSet<LocalUser> LocalUsers { get; set; }
```

Run the Entity Framework add migration tool with an appropriate name (e.g. AddLocalUsers).   
Then run update database.

### Add DTOs for Authentication

Add the following DTOs to the DTO folder -

A user DTO -

```C#
namespace MagicVilla_CouponAPI.Models.DTO;

public class UserDTO
{
    public int ID { get; set; }
    public string UserName { get; set; }
    public string Name { get; set; }
}
```

A registration request DTO -

```C#
namespace MagicVilla_CouponAPI.Models.DTO;

public class RegistrationRequestDTO
{
    public string UserName { get; set; }
    public string Name { get; set; }
    public string Password { get; set; }
}
```

A login request DTO -

```C#
namespace MagicVilla_CouponAPI.Models.DTO;

public class LoginRequestDTO
{
    public string UserName { get; set; }
    public string Password { get; set; }
}
```

And a login response DTO -

```C#
namespace MagicVilla_CouponAPI.Models.DTO;

public class LoginResponseDTO
{
    public UserDTO User { get; set; }
    public string Token { get; set; }
}
```

### Add Repository

Create an interface for the authentication repository -

```C#
using MagicVilla_CouponAPI.Models.DTO;

namespace MagicVilla_CouponAPI.Repository;

public interface IAuthRepository
{
    bool IsUniqueUser(string username);
    Task<LoginResponseDTO> Login(LoginRequestDTO loginRequestDto);
    Task<UserDTO> Register(RegistrationRequestDTO requestDto);
}
```

Before we can add the repository logic we need to update the appsettings.json file with a secret key -

```json
"ApiSettings": {
    "Secret": "THIS IS USED TO SIGN AND VERIFY JWT TOKES, REPLACE IT WITH YOUR OWN SECRET"
  },
```

Now add the repository class file and inherit from the new interface.   
Via dependency injection add the ApplicationDbContext, AutoMapper and IConfiguration.   
Create a secretKey property and add this to the constructor to import it from appSettings.   
Finally, implement and complete the logic for the methods required by the interface -

```C#
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;
using AutoMapper;
using MagicVilla_CouponAPI.Data;
using MagicVilla_CouponAPI.Models;
using MagicVilla_CouponAPI.Models.DTO;
using Microsoft.IdentityModel.Tokens;

namespace MagicVilla_CouponAPI.Repository;

public class AuthRepository : IAuthRepository
{
    private readonly ApplicationDbContext _db;
    private readonly IConfiguration _configuration;
    private readonly IMapper _mapper;
    private string secretKey;
    
    public AuthRepository(ApplicationDbContext db, IMapper mapper, IConfiguration configuration)
    {
        _db = db;
        _mapper = mapper;
        _configuration = configuration;
        secretKey = _configuration.GetValue<string>("ApiSettings:Secret");
    }
    
    public bool IsUniqueUser(string username)
    {
        var user = _db.LocalUsers.FirstOrDefault(x => x.UserName == username);

        if (user == null)
        {
            return true;
        }
        
        return false;
    }

    public async Task<LoginResponseDTO> Login(LoginRequestDTO loginRequestDto)
    {
        var user = _db.LocalUsers.SingleOrDefault(x =>
            x.UserName == loginRequestDto.UserName && x.Password == loginRequestDto.Password);

        if (user == null)
        {
            return null;
        }

        var tokenHandler = new JwtSecurityTokenHandler();
        var key = Encoding.ASCII.GetBytes(secretKey);
        var tokenDescriptor = new SecurityTokenDescriptor
        {
            Subject = new ClaimsIdentity(new Claim[]
            {
                new Claim(ClaimTypes.Name, user.UserName), 
                new Claim(ClaimTypes.Role, user.Role)
            }),
            Expires = DateTime.UtcNow.AddDays(7),
            SigningCredentials =
                new SigningCredentials(new SymmetricSecurityKey(key), SecurityAlgorithms.HmacSha256Signature)
        };

        var token = tokenHandler.CreateToken(tokenDescriptor);
        LoginResponseDTO loginResponseDto = new()
        {
            User = _mapper.Map<UserDTO>(user),
            Token = new JwtSecurityTokenHandler().WriteToken(token)
        };

        return loginResponseDto;
    }

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
}
```

### Auth Endpoints

In the program.cs file the authentication repository to the services -

```C#
builder.Services.AddScoped<IAuthRepository, AuthRepository>();
```

Create a new file in the endpoints folder for the auth endpoints -

```C#
using System.Net;
using AutoMapper;
using FluentValidation;
using MagicVilla_CouponAPI.Models;
using MagicVilla_CouponAPI.Models.DTO;
using MagicVilla_CouponAPI.Repository;
using Microsoft.AspNetCore.Mvc;

namespace MagicVilla_CouponAPI.Endpoints;

public static class AuthEndpoints
{
    public static void ConfigureAuthEndpoints(this WebApplication app)
    {
        app.MapPost("/api/login", Login).WithName("Login").Accepts<LoginRequestDTO>("application/json")
            .Produces<APIResponse>(200).Produces(400);
        
        app.MapPost("/api/register", Register).WithName("Register").Accepts<RegistrationRequestDTO>("application/json")
            .Produces<APIResponse>(200).Produces(400);
    }

    private async static Task<IResult> Login(IAuthRepository _authRepo, LoginRequestDTO model)
    {
        APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };
        
        var loginResponse = await _authRepo.Login(model);
        if (loginResponse == null)
        {
            response.ErrorMessages.Add("Username or password is incorrect");
            return Results.BadRequest(response);
        }
        
        response.Result = loginResponse;
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.OK;
        return Results.Ok(response);
    }
    
    private async static Task<IResult> Register(IAuthRepository _authRepo, RegistrationRequestDTO model)
    {
        APIResponse response = new() { isSuccess = false, StatusCode = HttpStatusCode.BadRequest };

        bool ifUserNameIsUnique = _authRepo.IsUniqueUser(model.UserName);
        if (!ifUserNameIsUnique == null)
        {
            response.ErrorMessages.Add("Username already exists");
            return Results.BadRequest(response);
        }

        var registerResponse = await _authRepo.Register(model);
        if (registerResponse == null || string.IsNullOrEmpty(registerResponse.UserName))
        {
            return Results.BadRequest(response);
        }
        
        response.isSuccess = true;
        response.StatusCode = HttpStatusCode.OK;
        return Results.Ok(response);
    }
}
```

Back in the program.cs file, add ConfigureAuthEndpoints to the app, and also UseAuthentication and UseAuthorization -

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
builder.Services.AddScoped<IAuthRepository, AuthRepository>();
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

app.UseAuthentication();
app.UseAuthorization();

app.ConfigureCouponEndpoints();
app.ConfigureAuthEndpoints();
app.UseHttpsRedirection();

app.Run();
```

Note:
: Ensure that UseAuthentication is called before UseAuthorization

We now need to add an AddAuthentication service, but before we can do that we must install a new package -

* Microsoft.AspNetCore.Authentication.JwtBearer

And then add the following to the program.cs file - 

```C#
builder.Services.AddAuthentication(x =>
{
    x.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
}).AddJwtBearer(x =>
{
    x.RequireHttpsMetadata = false;
    x.SaveToken = true;
    x.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuerSigningKey = true,
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.ASCII.GetBytes(
            builder.Configuration.GetValue<string>("ApiSettings:Secret"))),
        ValidateIssuer = false,
        ValidateAudience = false
    };
});
builder.Services.AddAuthorization();
```

Add the required mapping to the MappingConfig file -

```C#
using AutoMapper;
using MagicVilla_CouponAPI.Models;
using MagicVilla_CouponAPI.Models.DTO;

namespace MagicVilla_CouponAPI;

public class MappingConfig : Profile
{
    public MappingConfig()
    {
        CreateMap<Coupon, CouponCreateDTO>().ReverseMap();
        CreateMap<Coupon, CouponUpdateDTO>().ReverseMap();
        CreateMap<Coupon, CouponDTO>().ReverseMap();
        CreateMap<LocalUser, UserDTO>().ReverseMap();
    }
}
```

To enable logging in to function in swagger, update the AddSwaggerGen service with the following -

```C#
builder.Services.AddSwaggerGen(option =>
{
    option.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
    {
        Description =
            "JWT Authorization header using the Bearer scheme. \r\n\r\n " +
            "Enter 'Bearer' [space] and then your token in the text input below.\r\n\r\n" +
            "Example: \"Bearer 12345abcdef\"",
        Name = "Authorization",
        In = ParameterLocation.Header,
        Type = SecuritySchemeType.ApiKey,
        Scheme = "Bearer"
    });
    option.AddSecurityRequirement(new OpenApiSecurityRequirement()
    {
        {
            new OpenApiSecurityScheme
            {
                Reference = new OpenApiReference
                {
                    Type = ReferenceType.SecurityScheme,
                    Id = "Bearer"
                },
                Scheme = "oauth2",
                Name = "Bearer",
                In = ParameterLocation.Header,
            },
            new List<string>()
        }
    });
});
```

The completed program.cs file -

```C#
using System.Text;
using FluentValidation;
using MagicVilla_CouponAPI;
using MagicVilla_CouponAPI.Data;
using MagicVilla_CouponAPI.Endpoints;
using MagicVilla_CouponAPI.Repository;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.EntityFrameworkCore;
using Microsoft.IdentityModel.Tokens;
using Microsoft.OpenApi.Models;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(option =>
{
    option.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
    {
        Description =
            "JWT Authorization header using the Bearer scheme. \r\n\r\n " +
            "Enter 'Bearer' [space] and then your token in the text input below.\r\n\r\n" +
            "Example: \"Bearer 12345abcdef\"",
        Name = "Authorization",
        In = ParameterLocation.Header,
        Type = SecuritySchemeType.ApiKey,
        Scheme = "Bearer"
    });
    option.AddSecurityRequirement(new OpenApiSecurityRequirement()
    {
        {
            new OpenApiSecurityScheme
            {
                Reference = new OpenApiReference
                {
                    Type = ReferenceType.SecurityScheme,
                    Id = "Bearer"
                },
                Scheme = "oauth2",
                Name = "Bearer",
                In = ParameterLocation.Header,
            },
            new List<string>()
        }
    });
});
builder.Services.AddScoped<ICouponRepository, CouponRepository>();
builder.Services.AddScoped<IAuthRepository, AuthRepository>();
builder.Services.AddDbContext<ApplicationDbContext>(option =>
    option.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddValidatorsFromAssemblyContaining<Program>();
builder.Services.AddAutoMapper(typeof(MappingConfig));
builder.Services.AddAuthentication(x =>
{
    x.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
}).AddJwtBearer(x =>
{
    x.RequireHttpsMetadata = false;
    x.SaveToken = true;
    x.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuerSigningKey = true,
        IssuerSigningKey = new SymmetricSecurityKey(Encoding.ASCII.GetBytes(
            builder.Configuration.GetValue<string>("ApiSettings:Secret"))),
        ValidateIssuer = false,
        ValidateAudience = false
    };
});
builder.Services.AddAuthorization();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseAuthentication();
app.UseAuthorization();

app.ConfigureCouponEndpoints();
app.ConfigureAuthEndpoints();
app.UseHttpsRedirection();

app.Run();
```

### Add Authentication Check to Existing Endpoints

To have an endpoint require the user to be authenticated, call the RequireAuthentication method on the endpoint -

```C#
app.MapGet("/api/coupon", GetAllCoupon)
            .WithName("GetCoupons").Produces<APIResponse>(200)
            .RequireAuthorization();
```

Alternatively, you can add the Authorize attribute to the method the endpoint calls -

```C#
[Authorize]
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

### Test in Swagger

You should now be able to run the app, and test the authorization in swagger. First use the login endpoint and copy
the returned guid. Paste the guid into the authentication setting in the top right, and then calling the endpoints
that have RequireAuthorization should work.