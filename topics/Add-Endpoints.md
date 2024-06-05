# Add Endpoints

### Get All Endpoint

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

### Get Single Entry Endpoint

```C#
[HttpGet("GetSingleUser/{userId}")]
public User GetSingleUser(int userId)
{
    string sql = @"
        SELECT Users.UserId,
               Users.FirstName,
               Users.LastName,
               Users.Email,
               Users.Gender,
               Users.Active
        FROM TutorialAppSchema.Users
            WHERE UserId = " + userId.ToString();
    User users = _dapper.LoadDataSingle<User>(sql);
    return users;
}
```