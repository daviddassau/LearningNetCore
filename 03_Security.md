# Security

### Creating The User Model
1. Inside the `Models` folder, create a new class called `User.cs`.
2. Then you want to create 4 properties: 
```C#
public int Id { get; set; }
public string Username { get; set; }
public byte[] PasswordHash { get; set; }
public byte[] PasswordSalt { get; set; }
```
3. Navigate to `DataContext.cs` and add a new `DbSet` for User called Users. Make sure you update your database with your new Model.

### The Repository Pattern
