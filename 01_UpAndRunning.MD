# Up and Running with .NET Core
This is getting set up with a new .NET Core project, using the command line and VS Code as the code editor.

### Creating a .NET Core API using the DotNetCli
- Start by making a new directory, or CD'ing into an existing directory.
- The command you want to use to spin up a new project is `dotnet new webapi -o NameOfYourApp.API -n NameOfYourApp.API`.
- You can then CD into `NameOfYourApp.API` and do a `code .` which will open your project in VS Code.

### Reviewing the project files
- In `launchSettings.json` go to around line 24 where it says `"applicationUrl"`, and edit it to 
read `"applicationUrl": "http://localhost:5000",`

### Running the application
- Open up the terminal window inside VS Code and type `dotnet run`. This will run the application.
- In your browser, type in `http://localhost:5000/api/values`. The page should display `["value1","value2"]`.

### Creating The First Model and DataContext
1. The first thing you want to do is create a new folder in the main directory called `Models`. This will hold your data models. Once you create the folder, right-click and add a new C# class and name it `Value`. In `Value.cs` you can put in some dummy fields for now, such as `public int Id { get; set; }`. In this particular instance, when we do our data migrations, we'll end up with a table called Value. So obviously in any other case when making your own project, you would name the C# class in the Models folder whatever you want that table to be called.
2. Then you want to create an additional folder in the main directory called `Data` and put a new C# class in it called `DataContext`. You want to add a few things to this file; it should end up looking like this:
```C#
using DatingApp.API.Models;
using Microsoft.EntityFrameworkCore;

namespace DatingApp.API.Data
{
    public class DataContext : DbContext
    {
        public DataContext(DbContextOptions<DataContext> options) : base (options) {}
        public DbSet<Value> Values { get; set; }
    }
}
```
3. In `Startup.cs` go to the method that starts with `public void ConfigureServices...`. Put the following line as the first line in the method: `services.AddDbContext<DataContext>(x => x.UseSqlite("Connectionstring"));`. You will need to do a nuget package install for Sqlite, so do a `Ctr + Shift + P` and type in "nuget". Then you want to type in `Microsoft.EntityFrameworkCore.Sqlite`, select the most up to date version, and you're GTG. 

### Creating the database using Entity Framework migrations
1. The first thing you want to do when you need to set up your database using EF Core, is navigate to the `appsettings.json` file. Basically, you want to add in your new connection string for your new database. In the following code, you'll notice that the data source is `DatingApp.db`. Obviously you want to name it whatever is appropriate for your project. The file should look like this:
```C#
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=DatingApp.db"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```
2. In your `Startup.cs` file, go back to where you put the ("Connectionstring") placeholder. The line should look something like this now:
```C#
services.AddDbContext<DataContext>(x => x.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));
```
3. Now you want to open up your terminal window inside VS Code, and type in `dotnet ef migrations add InitialCreate`. What this does is start the process of creating your new DB. After running this, you should have a Migrations folder in the solution explorer.
4. Before you can go ahead and create the new database, you will need 3 nuget packages: `SQLitePCLRaw.bundle_e_sqlite3`, `SQLitePCLRaw.bundle_sqlcipher`, and `SQLitePCLRaw.bundle_green`.
5. Back in the terminal, run the command `dotnet ef database update`. This will update your new database with the oppropriate information.
6. You should now be able to go into DB Browser for SQLite, and click the Open Database button. Navigate to where your project is, and click on the database file.
7. If you click on the Browse Data tab, you can now enter in rows of data this way, if you'd like. All you need to do is write in the data, and click Write Changes at the top.

### Retrieving Data from the Database
1. In your `ValuesController.cs`, write a new method above the first method.
