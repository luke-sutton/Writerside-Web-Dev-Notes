# Add Endpoints - Dapper

### Get

```C#
[HttpGet("GetUsers")]
public IEnumerable<User> GetUsers()
{
    string sql = @"
        SELECT Users.UserId,
               Users.FirstName,
               Users.LastName,
               Users.Email,
               Users.Gender,
               Users.Active
        FROM TutorialAppSchema.Users";
    IEnumerable<User> users = _dapper.LoadData<User>(sql);
    return users;
}
```

### Get Single

```C#
[HttpGet("GetSingleUser/{userId}")]
public User GetSingleUser(int userId)
{
    string sql = $@"
        SELECT Users.UserId,
               Users.FirstName,
               Users.LastName,
               Users.Email,
               Users.Gender,
               Users.Active
        FROM TutorialAppSchema.Users
        WHERE UserId = {userId}";
    User users = _dapper.LoadDataSingle<User>(sql);
    return users;
}
```

### Put

```C#
[HttpPut("EditUser")]
public IActionResult EditUser(User user)
{
    string sql = $@"
        UPDATE TutorialAppSchema.Users
            SET FirstName = '{user.FirstName}',
                LastName  = '{user.LastName}',
                Email     = '{user.Email}',
                Gender    = '{user.Gender}',
                Active    = '{user.Active}'
            WHERE UserId = {user.UserId}";
    if (_dapper.ExecuteSql(sql))
    {
        return Ok();
    }

    throw new Exception("Failed to update user");
}
```

### Post

```C#
[HttpPost("AddUser")]
public IActionResult AddUser(UserToAddDto user)
{
    string sql = $@"INSERT INTO TutorialAppSchema.Users(
        FirstName,
        LastName,
        Email,
        Gender,
        Active
    ) VALUES (
         '{user.FirstName}',
         '{user.LastName}',
         '{user.Email}',
         '{user.Gender}',
         '{user.Active}'
    )";
    if (_dapper.ExecuteSql(sql))
    {
        return Ok();
    }

    throw new Exception("Failed to add user");
}
```

### Delete

```C#
[HttpDelete("DeleteUser/{userId}")]
public IActionResult DeleteUser(int userId)
{
    string sql = $"DELETE FROM TutorialAppSchema.Users WHERE UserId = {userId}";

    if (_dapper.ExecuteSql(sql))
    {
        return Ok();
    }

    throw new Exception("Failed to delete user");
}
```