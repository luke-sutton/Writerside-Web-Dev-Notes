# Add a Connection String

Open the appsettings.json file -

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

Add a new object called ConnectionStrings, then a key to use to name the connection (e.g. DefaultConnection),
Then enter the details as the value -

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost; Database=DotNetCourseDatabase; Trusted_Connection=false; TrustServerCertificate=True; User Id=sa; Password=password1;"
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