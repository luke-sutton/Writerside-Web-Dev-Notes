# Basic CRUD MVC App

How to build a basic app with full CRUD implementation using MVC.

### Setup

OS: macOS   
IDE: Rider   
Framework: net 8.0   
Database IDE: DataGrip   
Database: SQL Server using Docker   
ORM: Entity Framework Core

## Create The Project

Launch Rider and select new solution.   
Select 'Put solution and project in the same directory' (If this is a standalone project).   
Enter a solution and project name.
Select 'Web App (Model-View-Controller' as the template.
Press Create.

You can now run the project which will be a very basic website with a header with links to a home and privacy pages,
plus a footer.

## Understanding the Project Files

The project files are organized in the following structure:

- csproj: To view, click on project name in solution explorer and press F4. This file defines the overall structure of
  the project, lists dependencies and can contain build instructions and build configurations.
- Properties: This folder usually stores settings that define project specifics, for example, assembly information.
- launchsettings.json: (Contained within the properties folder) This is a configuration file that is used by the .NET
  Core runtime for setting up the hosting
  environment during the development phase.
- wwwroot: Contains static files such as CSS, JavaScript, and images.
- Controllers: Contains the controllers that handle the incoming requests and define the actions to be performed.
- Models: Contains the data models used by the application.
- Views: Contains the HTML templates that define the user interface.
- appsettings.json: Contains configuration settings for the application.
- Program.cs: The main project file
