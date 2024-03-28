# How to setup a Vue and .Net app

### Update your software

Open a terminal window and update your software installed via homebrew

```Shell
brew update && brew upgrade && brew autoremove && brew cleanup
```

You can set an alias to shorten the required text for frequently used commands.

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

This will create a folder within your project folder called client,
which is the common name for the frontend part of an application.

### Create the backend application

In the same terminal window enter the following command -

```shell
dotnet new web -o server
```

This will create a folder within your project folder called server,
which is the common name for the backend part of an application.

### Initialise GIT

In the same terminal window enter the following command -

```shell
git init
```

This will create a git branch at the root of your project folder so that
both the frontend and backend are included.

### Running the project

To run the frontend of your application open the project folder with your IDE of choice,
then within the integrated terminal, change directory to your client folder,
install the required npm packages and then run the app, you can do this by running the
following commands -

```shell
cd client
npm install
npm run dev
```

To run the backend of your application, again open the project folder within your IDE of choice,
then within the integrated terminal run change directory to the server folder and start
the app -

```shell
cd server
dotnet run
```

Opening and running the projects from the root project folder rather than the separate client
and server folders allows you to have your full project (frontend and backend) in the same git
repository.