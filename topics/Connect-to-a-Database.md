# Connect to a Database

### Database Connection

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

### Add Database Connection Tools / Libraries

Install the following packages -

* Dapper
* AutoMapper
* Microsoft.Data.SqlClient

### Connect to the Database

Next we will create a Data Access Layer (DAL)

Create a new folder within your project called Data.   
Add a new file to this folder called DataContextDapper.cs

```C#
namespace DotNetAPI.Data
{
    class DataContextDapper
    {
    
    }
}
```

Add a private readonly property of type IConfiguration, and call it _config

Then add a constructor that accepts a parameter of type IConfiguration called config, and assigns its value
to the _config property -

```C#
private readonly IConfiguration _config;

public DataContextDapper(IConfiguration config)
{
    _config = config;
}
```

Explanation:
: The property defines a readonly field of type IConfiguration which can only be initialized at the
declaration or in the constructor. Our config object is then assigned to this property in the constructor.
This ensures that it cannot be reassigned to after construction.

Next we will create 4 functions -

```C#
public IEnumerable<T> LoadData<T>(string sql)
{
    IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
    return dbConnection.Query<T>(sql);
}
```

This function returns an IEnumerable of types T. It executes the passed SQL query and maps the results to the
generic type T. You typically use it to fetch multiple records from the database.

```C#
public T LoadDataSingle<T>(string sql)
{
    IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
    return dbConnection.QuerySingle<T>(sql);
}
```

This function returns a single instance of type T. It executes the passed SQL query and maps the result to T.
You use it when you expect your query to return precisely one row.

```C#
public bool ExecuteSql(string sql)
{
    IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
    return dbConnection.Execute(sql) > 0;
}
```

This function executes the passed SQL command (such as INSERT, UPDATE, or DELETE) and returns a boolean
indicating whether the operation affected any rows (true if at least one row was affected).

```C#
public int ExecuteSqlWithRowCount(string sql)
{
    IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
    return dbConnection.Execute(sql);
}
```

Similar to ExecuteSql, but instead of a boolean, this function returns the number of rows affected by the SQL command.

These functions abstract the process of creating a connection to the database, executing a statement,
and closing the connection. They make your code cleaner and easier to understand by encapsulating the
boilerplate code needed to interact with the database.

Here is the completed file -

```C#
using System.Data;
using Dapper;
using Microsoft.Data.SqlClient;

namespace DotNetAPI.Data
{
    class DataContextDapper
    {
        private readonly IConfiguration _config;

        public DataContextDapper(IConfiguration config)
        {
            _config = config;
        }

        public IEnumerable<T> LoadData<T>(string sql)
        {
            IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
            return dbConnection.Query<T>(sql);
        }
        
        public T LoadDataSingle<T>(string sql)
        {
            IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
            return dbConnection.QuerySingle<T>(sql);
        }

        public bool ExecuteSql(string sql)
        {
            IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
            return dbConnection.Execute(sql) > 0;
        }
        
        public int ExecuteSqlWithRowCount(string sql)
        {
            IDbConnection dbConnection = new SqlConnection(_config.GetConnectionString("DefaultConnection"));
            return dbConnection.Execute(sql);
        }
    }
}
```

### Test the Connection

Within your controller, add a property of type DataContextDapper, then add a constructor and assign it
to a new DataContextDapper object and pass in config -

```C#
private DataContextDapper _dapper;
    
public UserController(IConfiguration config)
{
    _dapper = new DataContextDapper(config);
}
```

Then create a new endpoint to test the connection -

```C#
[HttpGet("TestConnection")]
public DateTime TestConnection()
{
    return _dapper.LoadDataSingle<DateTime>("SELECT GETDATE()");
}
```