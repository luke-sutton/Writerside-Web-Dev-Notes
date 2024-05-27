# Insert, Update, Delete and Search

### Inserting a Row into a Table

To add a new row to a table use the insert into statement, followed by the table name, the values to be updated,
then use the values statement and add the values to be added -

```SQL
INSERT INTO TutorialAppSchema.Computer (Motherboard, CPUCores, HasWifi, HasLTE, ReleaseDate, Price, VideoCard)
VALUES ('Sample-Motherboard', 4, 1, 0, '2022-01-01', 1000, 'Sample-Videocard')
```

### Updating a Row in a Table

To update a row in a table use the update statement followed by the table name, then the set statement
the column name to be updated and the new value, then a where statement followed by details of the row
to update -

```SQL
UPDATE TutorialAppSchema.Computer SET CPUCores = 8 WHERE computerId = 2
```

If your where statement returns multiple rows, all will be updated with the new value -

```SQL
UPDATE TutorialAppSchema.Computer SET CPUCores = 8 WHERE ReleaseDate < '2017-01-01'
```

If you do not add a where statement all rows will be updated with the new value -

```SQL
UPDATE TutorialAppSchema.Computer SET CPUCores = 8
```

### Deleting a Row From a Table

To delete a row from a table use the delete from statement followed by the table name, then the where
statement, then enter a way to identify the row to be deleted.

Using the primary key with a unique identifier is a good way of identifying the row you wish to delete -

```SQL
DELETE FROM TutorialAppSchema.Computer WHERE computerId = 1
```

### Searching Data in a Table

You can search data in a table using the select statement followed by the columns you wish to see then
the from statement and the name of the database -

```SQL
SELECT computerId,
       Motherboard,
       CPUCores,
       HasWifi,
       HasLTE,
       ReleaseDate,
       Price,
       VideoCard
FROM TutorialAppSchema.Computer
```

If you wish to see all columns you can use an asterisk which represents all -

```SQL
SELECT * FROM TutorialAppSchema.Computer
```

To find a specific entry / entries you can add the where statement with the details you wish to see -

```SQL
SELECT * FROM TutorialAppSchema.Computer WHERE computerId = 2
```

You can order the results by using the order by statement followed by the conditions -

```SQL
SELECT * FROM TutorialAppSchema.Computer
ORDER BY ReleaseDate
```

You can reverse the order by adding the desc statement -

```SQL
SELECT * FROM TutorialAppSchema.Computer
ORDER BY ReleaseDate DESC
```

And multiple conditions can be used -

```SQL
SELECT * FROM TutorialAppSchema.Computer
ORDER BY HasWifi DESC, ReleaseDate DESC
```

You can display alternative results for data in columns by using a modifier like isnull then passing
in the column name and the value you would like displayed -

```SQL
SELECT computerId,
       Motherboard,
       ISNULL(CPUCores, 2),
       HasWifi,
       HasLTE,
       ReleaseDate,
       Price,
       VideoCard
FROM TutorialAppSchema.Computer
```

In the above example any rows that have null as an entry for CPUCores will display 2 instead of null.

This will cause the column name to change to no column name or anonymous.   
But you can give it a name by using an alias -

```SQL
SELECT computerId,
       Motherboard,
       ISNULL(CPUCores, 2) AS CPUCores,
       HasWifi,
       HasLTE,
       ReleaseDate,
       Price,
       VideoCard
FROM TutorialAppSchema.Computer
```