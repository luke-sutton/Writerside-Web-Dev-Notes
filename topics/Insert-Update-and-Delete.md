# Insert, Update and Delete

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

### Insert a Column Into a Table

You can add a new column into an existing table using alter table.

FIrst use the alter table statement followed by the tables name,   
Teh use the add statement followed by the details of the column to be added -

```SQL
ALTER TABLE TutorialAppSchema.UserSalary ADD AvgSalary DECIMAL(18,4) NULL
```