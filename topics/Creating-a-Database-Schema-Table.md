# Creating a Database, Schema &amp; Table

### Creating & Connecting to a Database

To create a new database use CREATE DATABASE followed by the name you wish to call the database -

```SQL
CREATE DATABASE DotNetCourseDatabase
```

To connect to the database you use the use statement followed by the name of the database you wish to use -

```SQL
USE DotNetCourseDatabase
```

### Creating a Schema

To create a new schema use the create schema statement followed by the name for the schema -

```SQL
CREATE SCHEMA TutorialAppSchema
```

### Creating a Table

To create a new table use the create table statement followed by the schema and name for the table -

```SQL
CREATE TABLE  TutorialAppSchema.Computer
(

)
```

Next add a column whose purpose is for indexing the data -

```SQL
computerId INT IDENTITY(1,1) PRIMARY KEY
```

First enter the columns name followed by the data type e.g. int.   
Next add the identity statement which specifies that this column is to be used for sorting. Pass in 2 numbers,
the first is the starting number and the second is how much the number should increment by for each entry.   
Finally, add the primary key statement, this ensures that each entry created will be unique.

Next add the columns required to the database including the data types, it is also considered best practice to state
if the data should be nullable by adding either the statement null or not null -

```SQL
Motherboard NVARCHAR(50) NULL,
CPUCores INT NULL,
HasWifi BIT NULL,
HasLTE BIT NULL,
ReleaseDate DATETIME NULL,
Price DECIMAL(18, 4) NULL,
VideoCard NVARCHAR(50) NULL
```

So the full instruction will be as follows -

```SQL
CREATE TABLE  TutorialAppSchema.Computer
(
    computerId INT IDENTITY(1,1) PRIMARY KEY,
    Motherboard NVARCHAR(50) NULL,
    CPUCores INT NULL,
    HasWifi BIT NULL,
    HasLTE BIT NULL,
    ReleaseDate DATETIME NULL,
    Price DECIMAL(18, 4) NULL,
    VideoCard NVARCHAR(50) NULL
)
```

### Running Statements as a Script

A script is a set of statements that are executed in sequence.

It is considered best practice to use the go statement between unrelated statements when running a script.   
This informs the computer that everything run after this statement is a new and separate query.

For example the script for the work we have completed so far would look like this -

```SQL
CREATE DATABASE DotNetCourseDatabase
GO

USE DotNetCourseDatabase
GO

CREATE SCHEMA TutorialAppSchema
GO

CREATE TABLE  TutorialAppSchema.Computer
(
    computerId INT IDENTITY(1,1) PRIMARY KEY,
    Motherboard NVARCHAR(50) NULL,
    CPUCores INT NULL,
    HasWifi BIT NULL,
    HasLTE BIT NULL,
    ReleaseDate DATETIME NULL,
    Price DECIMAL(18, 4) NULL,
    VideoCard NVARCHAR(50) NULL
)
```