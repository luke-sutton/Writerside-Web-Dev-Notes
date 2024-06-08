# AutoMapper

AutoMapper is a widely used object-object mapping library in .NET. Its primary goal is to reduce the complexity and
boilerplate code in a project by automatically copying data from one object type to another.
In essence, AutoMapper automates the process of transferring data between two objects that share similar properties,
removing the need to manually write code to carry out this operation.

### Install the required library

* AutoMapper

### Update the controller

In the controller add a property called _mapper of type IMapper -

```C#
IMapper _mapper;
```

Then in your constructor, set the value of _mapper to a new Mapper object and pass in a new MapperConfiguration
object, and pass into that a lambda function with an input parameter of cfg an in the expression body call CreateMap
and specify the models to be mapped from and to -

```C#
_mapper = new Mapper(new MapperConfiguration(cfg =>
{
    cfg.CreateMap<UserToAddDto, User>();
}));
```

### Create the Endpoint

Here is a AddUser enpoint using Dapper -

```C#
[HttpPost("AddUser")]
public IActionResult AddUser(UserToAddDto user)
{
    User userDb = new User();
    
    userDb.Active = user.Active;
    userDb.FirstName = user.FirstName;
    userDb.LastName = user.LastName;
    userDb.Email = user.Email;
    userDb.Gender = user.Gender;
    _entityFramework.Add(userDb);
    if (_entityFramework.SaveChanges() > 0)
    {
        return Ok();
    } 

    throw new Exception("Failed to Add User");
}
```

To update it to use AutoMapper, Instead of assigning userDb to a new User object, you assign it to _mapper, call the
Map method, specify the type we want the result to be of and pass in the argument to be mapped to that type.

```C#
User userDb = _mapper.Map<User>(user);
```

So in this new method the passed in object 'user' will be mapped to object type 'User' and the result assigned to
userDb.


The previous assignments can then be removed so the completed endpoint will look like this -

```C#
[HttpPost("AddUser")]
public IActionResult AddUser(UserToAddDto user)
{
    User userDb = _mapper.Map<User>(user);
    
    _entityFramework.Add(userDb);
    if (_entityFramework.SaveChanges() > 0)
    {
        return Ok();
    } 

    throw new Exception("Failed to Add User");
}
```