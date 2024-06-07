# Connect the DAL and Controller

### Inject the DAL into the controller

Within your controller, add a property of the type of whatever your DAL is called (e.g. DataContextDapper or
DataContextEF), then add a constructor and assign it to a new DAL object and pass in config -

```C#
private DataContextDapper _dapper;
    
public UserController(IConfiguration config)
{
    _dapper = new DataContextDapper(config);
}
```

Example using Entity Framework -

```C#
private DataContextEF _entityFramework;

public UserEFController(IConfiguration config)
{
    _entityFramework = new DataContextEF(config);
}
```

### Test the Connection

Then create a new endpoint to test the connection -

```C#
[HttpGet("TestConnection")]
public DateTime TestConnection()
{
    return _dapper.LoadDataSingle<DateTime>("SELECT GETDATE()");
}
```

Note:
: _dapper is named with an underscore as this is a common c# naming convention for private fields.
This helps to quickly differentiate between private fields and local parameters or variables