# Up and Running with .NET Core
This is getting set up with a new .NET Core project, using the command line and VS Code as the code editor.

### Creating a .NET Core API using the DotNetCli
- Start by making a new directory, or CD'ing into an existing directory.
- The command you want to use to spin up a new project is `dotnet new webapi -o NameOfYourApp.API -n NameOfYourApp.API`.
- You can then CD into `NameOfYourApp.API` and do a `code .` which will open your project in VS Code.

### Reviewing the project files
- In `launchSettings.json` go to around line 24 where it says `"applicationUrl"`, and edit it to 
read `"applicationUrl": "http://localhost:5000",`
