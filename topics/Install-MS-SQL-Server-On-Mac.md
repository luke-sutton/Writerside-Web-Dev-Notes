# Install MS SQL Server On Mac

In order to run MS SQL Server on a Mac it must be run in a docker container.

The version we will be using is called Azure SQL Edge.

Enter the following in the command line to download the container -

Note:
: The name assigned to MSSQL_USER is your username. The SA used below is a common used name for system admin.   
The value for MSSQL_SA_PASSWORD is your password, the currently entered asterisks should be replaced with
a secure password of your choice.

```Bash
docker run -e "ACCEPT_EULA=1" -e "MSSQL_USER=SA" -e "MSSQL_SA_PASSWORD=****" -e "MSSQL_PID=Developer" -p 1433:1433 -d --name=sql_connect mcr.microsoft.com/azure-sql-edge
```

To check the container is running -

```Bash
docker container ls -a
```

To stop and start container -

```Bash
docker stop sql_connect

docker start sql_connect
```

The container can also be stopped and started within the Docker GUI.

### Connecting to the Database

In your database management system (such as Azure Data Studio or DataGrip etc.) enter the following parameters -

If using DataGrip select Azure SQL Database as the database type.

Server / host: localhost

Authentication: If using Azure Data Studio select SQL Logging, if using DataGrip select User & Password

Port: 1433

User / User name: your chosen user name (e.g. SA)

Password: your chosen password

If you are using Azure Data Studio after connecting select the option to trust the certificate.   
If you are using DataGrip go to the advanced settings and set encrypt to false.

### Test the Connection

A common way of testing that you are successfully connected to a database is by creating a new query
and entering the following -

```SQL
SELECT GETDATE()
```

If the current data and time are returned then you know that the connection was successful.