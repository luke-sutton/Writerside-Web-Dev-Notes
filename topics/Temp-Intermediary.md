# Alias, Join and Where Exists

### Aliases

You can give a table an alias using the as statement -

```SQL
SELECT * FROM TutorialAppSchema.Users AS Users
```

This is referred to as a table alias.

Note:
: In Azure Data studio expanding the list (place cursor after * and press ctrl + space) when an alias
is present will create a qualified list
```SQL
SELECT [Users].[UserId],
[Users].[FirstName],
[Users].[LastName],
[Users].[Email],
[Users].[Gender],
[Users].[Active] FROM TutorialAppSchema.Users AS Users
```
When using DataGrip you do not need to add an alias first, just expand the list then highlight it
and select qualify. (select * and press option + enter)

Columns can be combined and given a new name using as alias, in the example below the FirstName and LastName
columns are concatenated and the column renamed to FullName -

```SQL
SELECT Users.UserId,
       Users.FirstName + ' ' + Users.LastName AS FullName,
       Users.Email,
       Users.Gender,
       Users.Active
FROM TutorialAppSchema.Users AS Users
```

This is referred to as a field or value alias.

### Join

Join can be used to combine the results or 2 or more tables into one -

```SQL
SELECT Users.UserId,
       Users.FirstName + ' ' + Users.LastName AS FullName,
       Users.Email,
       Users.Gender,
       Users.Active,
       UserJobInfo.JobTitle,
       UserJobInfo.Department
FROM TutorialAppSchema.Users AS Users
    JOIN TutorialAppSchema.UserJobInfo
    ON UserJobInfo.UserId = Users.UserId
WHERE Users.Active = 1
ORDER BY Users.UserId DESC
```

Here is a breakdown of what is happening in the above example which shows a table called Users
and a table called UserJobInfo joined together using a column called UserId that they both
have in common -

An order by statement has been added to order the results in descending order by the UserId column.   
A where statement is used to only return rows where the active column in the Users table is set to true.   
The columns from the UserJobInfo table have been added to the list, this was achieved by adding UserJobInfo.*
to the list and then expanding the list. This also added a UserJobInfo.UserId column, but as this is the column
that is shared by both tables one of them has been removed.   
The join statement was added after the from statement to state that we wish to add the UserJobInfo tables data
and columns.   
The on statement was added to state how they should be combined, which in this case is the shared UserId column.

Multiple join statements can be used to combine multiple tables results -

```SQL
SELECT Users.UserId,
       Users.FirstName + ' ' + Users.LastName AS FullName,
       Users.Email,
       Users.Gender,
       Users.Active,
       UserJobInfo.JobTitle,
       UserJobInfo.Department,
       UserSalary.Salary
FROM TutorialAppSchema.Users AS Users
    JOIN TutorialAppSchema.UserSalary
    ON UserSalary.UserId = Users.UserId
        JOIN TutorialAppSchema.UserJobInfo
        ON UserJobInfo.UserId = Users.UserId
WHERE Users.Active = 1
ORDER BY Users.UserId DESC
```

### Where Exists

The where exists clause only returns data when a subquery returns true.

Basic syntax -

```SQL
SELECT column1, column2, ...
FROM table1
WHERE EXISTS (subquery);
```

So here the rows for columns 1 and 2 from table 1 will only be returned if the subquery statement is true.

```SQL
SELECT UserSalary.UserId,
       UserSalary.Salary
FROM TutorialAppSchema.UserSalary
WHERE EXISTS(
        SELECT * FROM TutorialAppSchema.UserJobInfo AS UserJobInfo
                 WHERE UserJobInfo.UserId = UserSalary.UserId
    )
```

In the above example the columns will only be returned iif there is a matching UserId in the UserJobInfo
table for each UserId in the UserSalary table.

The advantage of using a where exists clause over a join is that it is much faster.