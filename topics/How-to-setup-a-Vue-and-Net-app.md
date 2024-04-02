# How to setup a Vue and .Net app

### Update your software

Ensure any relevant software is up-to-date, for example you have the latest LTS version of Node installed.

### Create a folder for your new project

Create a new folder in the location you would like to keep the project files
and name the folder the title of your project.

```shell
cd Dev
mkdir "project name"
```

### Create the frontend application

In a terminal window pointing to the folder of your new project
enter the following command -

```shell
npm create vue@latest
```

When it asks you for a project name enter 'client' and then select the relevant
options for your project.

Note:
: The name client can be replaced with any you wish, but client is the most common name given for the frontend
part of an application like this.

This will create a folder within your project folder called client,
which is the common name for the frontend part of an application.

### Create the backend application

In the same terminal window enter the following command -

```shell
dotnet new web -o server
```

This will create a folder within your project folder called server,

Note:
: The name server can be replaced with any you wish, but server is the most common name given for the backend
part of an application like this.

### Initialise GIT

In a terminal window, cd into the project folders and enter the following command -

```shell
git init
```

If you cd into the main project folder this will create a git branch that will track and monitor both frontend
and backend apps. If you prefer to keep them separate run the command within both the client and server
folders.

### Running the project

To run the frontend of your application open the client folder with your IDE of choice.
install the required npm packages (only required the first time you run) and then run the app, you can do
this by running the following commands -

```shell
npm install
npm run dev
```

To run the backend of your application, open the server folder within your IDE of choice,
then within the integrated terminal run the following to start the app -

```shell
dotnet run
```
