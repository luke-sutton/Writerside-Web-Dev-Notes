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

Next add the first entry into the table -

```SQL
CREATE TABLE  TutorialAppSchema.Computer
(
    computerId INT IDENTITY(1,1) PRIMARY KEY
)
```

In the above example computerId is the name given to the tableId, INT is the type of data. The identity statement
means that this is not an entry that we will be adding data to, Its purpose is for sorting the other entries. The first
number in the brackets is the starting value (this is referred to as the seed), and the second value is how much it
should increase by each time an entry is added to the database (this is referred to as the increment).
The use of primary key at the end ensures that the id's created are always unique.

Next add entries to the database including the data types -

```SQL
Motherboard NVARCHAR(50),
CPUCores INT,
HasWifi BIT,
HasLTE BIT,
ReleaseDate DATETIME,
Price DECIMAL(18, 4),
VideoCard NVARCHAR(50)
```

So the full instruction will be as follows -

```SQL
CREATE TABLE  TutorialAppSchema.Computer
(
    computerId INT IDENTITY(1,1) PRIMARY KEY,
    Motherboard NVARCHAR(50),
    CPUCores INT,
    HasWifi BIT,
    HasLTE BIT,
    ReleaseDate DATETIME,
    Price DECIMAL(18, 4),
    VideoCard NVARCHAR(50)
)
GO
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
    Motherboard NVARCHAR(50),
    CPUCores INT,
    HasWifi BIT,
    HasLTE BIT,
    ReleaseDate DATETIME,
    Price DECIMAL(18, 4),
    VideoCard NVARCHAR(50)
)
```