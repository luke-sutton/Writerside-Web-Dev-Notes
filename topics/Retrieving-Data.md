# Retrieving Data

You can retrieve data in a table using the select statement followed by an asterisk (which represents all
columns) then add the from statement and the name of the schema and table -

```SQL
SELECT * FROM TutorialAppSchema.Computer
```

The table should then be given an alias using the as statement (The name of the table is usually used 
for this) -

```SQL
SELECT * FROM TutorialAppSchema.Computer AS Computer
```

Even if you wish to retrieve all data it is considered bad practice to leave the asterisk, and you should instead
expand and qualify the list (qualify means to add the name of the table before each column name) -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       Computer.CPUCores,
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer AS Computer
```

Note:
: When using DataGrip expand the list (select * and press option + enter) then highlight the entries
and select qualify.   
In Azure Data studio expanding the list (place cursor after * and press ctrl + space) when an alias
is present will create a qualified list.

### Retrieving Specific Data

To find a specific entry / entries you can add the where statement with the details you wish to see -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       Computer.CPUCores,
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer AS Computer WHERE computerId = 2
```

You can check if data exists and then only return it if it does, using where exists.

```SQL
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```

```SQL
SELECT UserSalary.UserId,
       UserSalary.Salary
FROM TutorialAppSchema.UserSalary
WHERE EXISTS(SELECT *
             FROM TutorialAppSchema.UserJobInfo AS UserJobInfo
             WHERE UserJobInfo.UserId = UserSalary.UserId)
```

If the condition within the where exists clause is true then the data is returned.

### Ordering the Results

You can order the results by using the order by statement followed by the conditions -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       Computer.CPUCores,
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer AS Computer WHERE computerId = 2
ORDER BY ReleaseDate
```

You can reverse the order by adding the desc statement -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       Computer.CPUCores,
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer AS Computer WHERE computerId = 2
ORDER BY ReleaseDate DESC
```

And multiple conditions can be used -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       Computer.CPUCores,
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer AS Computer WHERE computerId = 2
ORDER BY HasWifi DESC, ReleaseDate DESC
```

### Modify the Results

You can display alternative results for data in columns by using a modifier like isnull then passing
in the column name and the value you would like displayed -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       ISNULL(CPUCores, 2),
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer AS Computer WHERE computerId = 2
```

In the above example any rows that have null as an entry for CPUCores will display 2 instead of null.

This will cause the column name to change to no column name or anonymous.   
But you can give it a name by using an alias -

```SQL
SELECT Computer.computerId,
       Computer.Motherboard,
       ISNULL(CPUCores, 2), AS CPUCores
       Computer.HasWifi,
       Computer.HasLTE,
       Computer.ReleaseDate,
       Computer.Price,
       Computer.VideoCard
FROM TutorialAppSchema.Computer
```

Columns can be combined and given a new name using an alias, in the example below the FirstName and LastName
columns are concatenated and the column renamed to FullName -

```SQL
SELECT Users.UserId,
       Users.FirstName + ' ' + Users.LastName AS FullName,
       Users.Email,
       Users.Gender,
       Users.Active
FROM TutorialAppSchema.Users AS Users
```

### Joining Retrieved Data

The results of multiple tables can be retrieved and combined into one set of results.

The join statement was added after the from statement to state that we wish to add the
UserJobInfo tables data and columns. The on statement was added to state how they should be combined,
which in this case is the shared UserId column.

First add the columns from the new table you wish to include to the qualified list,   
Then add a join statement with the name of the table whose data you wish to add to the results.
Then add an on statement to state how they should be combined -

```SQL
SELECT Users.UserId,
       Users.FirstName,
       Users.LastName,
       Users.Email,
       Users.Gender,
       Users.Active,
       UserJobInfo.JobTitle,
       UserJobInfo.Department
FROM TutorialAppSchema.Users AS Users
         JOIN TutorialAppSchema.UserJobInfo
              ON UserJobInfo.UserId = Users.UserId
```

So in this example the JobTitle and Department columns from the UserJobInfo table will be added
to the retrieved data from the Users table, and they will be paired together based on the UserId column.

The data from more than 2 tables can be combined using multiple joins -

```SQL
SELECT Users.UserId,
SELECT Users.UserId,
       Users.FirstName,
       Users.LastName,
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
```