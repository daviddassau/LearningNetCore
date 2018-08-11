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
1. The first thing you want to do is create a new folder in the main directory called `Models`. This will hold your data models. Once you create the folder, right-click and add a new C# class and name it `Value`. In `Value.cs` you can put in some dummy fields for now, such as `public int Id { get; set; }`.
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
