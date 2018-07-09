# Up and Running with Entity Framework Core

### 1. Setting up EF Core
The first thing you need to do after creating your .NET Core project is to install Entity Framework Core. You can do this one of two ways: the nu-get package manager or the package manager console.

#### Installing via Nu-Get Package Manager
1. Go to the package manager, and browse for `microsoft.entityframeworkcore`.
2. Install the one titled `Microsoft.EntityFrameworkCore.SqlServer`.

#### Installing via Nu-Get Package Manager Console
1. Install-Package Microsoft.EntityFrameworkCore.SqlServer

The second thing you need is to install EF Tools in order to execute EF Core commands. These make it easier to perform several EF Core-related tasks in your project at design time, such as migrations, scaffolding, etc.

#### Installing via Nu-Get Package Manager
1. Go to the package manager, and browse for `Microsoft.EntityFrameworkCore.Tools`.

#### Installing the Scaffolding Tools
1. Go to the package manager, and browse for `Microsoft.VisualStudio.Web.CodeGeneration.Design`.

### 2. Set up DB
Navigate to your `appsettings.json` (create one if you don't have one already), and paste in the following:
```JSON
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(local);Database=NameOfYourDatabase;Trusted_Connection=True;"
  }
}
```
Then, you want to add your DB Context Model:
1. Add folder to project called `Models` (if you don't have one yet).
2. Add your db context model class (you can call it `Model.cs`) to the `Model` folder.
3. Add the following to that file:
```C#
public class Model : DbContext
    {
        public Model(DbContextOptions<Model> options)
            : base(options)
        {
        }
    }
```
