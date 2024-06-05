# Connect the DAL and Controller

### Inject the DAL into the controller

Within your controller, add a property of type DataContextDapper, then add a constructor and assign it
to a new DataContextDapper object and pass in config -

```C#
private DataContextDapper _dapper;
    
public UserController(IConfiguration config)
{
    _dapper = new DataContextDapper(config);
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