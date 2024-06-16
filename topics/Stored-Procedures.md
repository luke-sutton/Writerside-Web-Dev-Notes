# Stored Procedures

Stored procedures hold SQL queries, which allows them to be called and run multiple times without needing to rewrite
the query

Sored procedures are created using CREATE PROCEDURE followed by the database name, then the name you wish to call it -

```SQL
CREATE PROCEDURE TutorialAppSchema.spUsers_Get
```

Note:
: sp is commonly used at the beginning of the stored procedures name

Next you follow this with an AS command followed by BEGIN and END -

```SQL
CREATE PROCEDURE TutorialAppSchema.spUsers_Get
AS 
    BEGIN 
        
    END 
```

You will then add your queries between the begin and end commands -

```SQL
USE DotNetCourseDatabase
GO

CREATE PROCEDURE TutorialAppSchema.spUsers_Get
AS 
    BEGIN 
        SELECT GETDATE()
    END 
```

Then you can call the stored procedure using the EXEC command -

```SQL
EXEC TutorialAppSchema.spUsers_Get
```

Note:
: Some people like to add this command as a comment under the create procedure line -
```SQL
CREATE PROCEDURE TutorialAppSchema.spUsers_Get
-- EXEC TutorialAppSchema.spUsers_Get
AS 
    BEGIN 
        SELECT GETDATE()
    END 
```
This way this there is a quick way to run and access the procedure when you are working on it and making
changes

If you make changes to the logic in the procedure and try to run it again, you will receive an error stating
that the procedure already exists

So after making changes you need to use the ALTER command instead of CREATE -

```SQL
ALTER PROCEDURE TutorialAppSchema.spUsers_Get
-- EXEC TutorialAppSchema.spUsers_Get
AS
BEGIN
    SELECT Users.UserId,
           Users.FirstName,
           Users.LastName,
           Users.Email,
           Users.Gender,
           Users.Active
    FROM TutorialAppSchema.Users AS Users
END 
```

### Parameters

Parameters can be added by creating variables using the @ symbol, and placing them at the top of the procedure
before the AS command -

```SQL
@UserId INT
```

We can then reference this in our query

```SQL
ALTER PROCEDURE TutorialAppSchema.spUsers_Get
    @UserId INT
AS
BEGIN
    SELECT Users.UserId,
           Users.FirstName,
           Users.LastName,
           Users.Email,
           Users.Gender,
           Users.Active
    FROM TutorialAppSchema.Users AS Users
    Where Users.UserId = @UserId
END 
```

In the above example where we have declared a variable of type INT for the UserId, so when the stored procedure is run
with one supplied the single entry will be returned.

To use the parameters they just need to be added to  the end of the EXEC statement -

```SQL
EXEC TutorialAppSchema.spUsers_Get 3
```

When using them in this way, if multiple parameters are required, they must be listed (and separated by a comma) in
the order they are written in the query

While the above works fine it is more common to see them written more explicitly like so -

```SQL
EXEC TutorialAppSchema.spUsers_Get @UserId=3
```

When this method is used the parameters order does not matter

You can assign a default value to a parameter -

```SQL
 @UserId INT = 3
```

This way if the procedure is called without the parameter supplied, it will still run and return the result using
the default value

You can make a parameter nullable by assigning NULL to its value -

```SQL
 @UserId INT = NULL
```

This can be used along with an ISNULL check in the logic to return a single entry if an id is supplied, or
return all entries if not -

```SQL
ALTER PROCEDURE TutorialAppSchema.spUsers_Get
    @UserId INT = NULL
AS
BEGIN
    SELECT Users.UserId,
           Users.FirstName,
           Users.LastName,
           Users.Email,
           Users.Gender,
           Users.Active
    FROM TutorialAppSchema.Users AS Users
    Where Users.UserId = ISNULL(@UserId, Users.UserId)
END 
```