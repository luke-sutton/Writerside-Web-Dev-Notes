# Add Endpoints - Entity Framework

### Get

```C#
[HttpGet("GetUsers")]
public IEnumerable<User> GetUsers()
{
    IEnumerable<User> users = _entityFramework.Users.ToList<User>();
    return users;
}
```

### Get Single

```C#
[HttpGet("GetSingleUser/{userId}")]
public User GetSingleUser(int userId)
{
    User? user = _entityFramework.Users
        .Where(u => u.UserId == userId)
        .FirstOrDefault<User>();

    if (user != null)
    {
        return user;
    }
    
    throw new Exception("Failed to Get User");
}
```

### Put

```C#
[HttpPut("EditUser")]
public IActionResult EditUser(User user)
{
    User? userDb = _entityFramework.Users
        .Where(u => u.UserId == user.UserId)
        .FirstOrDefault<User>();
        
    if (userDb != null)
    {
        userDb.Active = user.Active;
        userDb.FirstName = user.FirstName;
        userDb.LastName = user.LastName;
        userDb.Email = user.Email;
        userDb.Gender = user.Gender;
        if (_entityFramework.SaveChanges() > 0)
        {
            return Ok();
        } 

        throw new Exception("Failed to Update User");
    }
    
    throw new Exception("Failed to Get User");
}
```

### Post

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

### Delete

```C#
[HttpDelete("DeleteUser/{userId}")]
public IActionResult DeleteUser(int userId)
{
    User? userDb = _entityFramework.Users
        .Where(u => u.UserId == userId)
        .FirstOrDefault<User>();
        
    if (userDb != null)
    {
        _entityFramework.Users.Remove(userDb);
        if (_entityFramework.SaveChanges() > 0)
        {
            return Ok();
        } 

        throw new Exception("Failed to Delete User");
    }
    
    throw new Exception("Failed to Get User");
}
```