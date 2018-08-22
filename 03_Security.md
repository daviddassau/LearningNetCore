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
1. In the `Data` folder, create a new interface called `IAuthRepository`.
2. You're going to support 3 different methods inside of this interface:
  - Registering the user
  - User login to the API
  - Check if the user exists
3. The interface should look similar to this:
```C#
public interface IAuthRepository
{
    Task<User> Register(User user, string password);
    Task<User> Login(string username, string password);
    Task<bool> UserExists(string username);
}
```
