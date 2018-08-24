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

### Creating the Concrete Auth Repository and Register Method
1. Create a new C# class in the `Data` folder called `AuthRepository`
2. Let it inherit the `IAuthRepository`. You will get a red-line, just CTRL+. and say "Implement interface".
3. Above the newly generated code, as the new method(s), insert the following snippet:
```C#
private readonly DataContext _context;
public AuthRepository(DataContext context)
{
    _context = context;
}
```
4. Within the `Register` method, delete the `throw new...` and replace it with:
```C#
byte[] passwordHash, passwordSalt;
CreatePasswordHash(password, out passwordHash, out passwordSalt);

user.PasswordHash = passwordHash;
user.PasswordSalt = passwordSalt;

await _context.Users.AddAsync(user);
await _context.SaveChangesAsync();

return user;
```
5. The `CreatePasswordHash` part will be red-lined, so go ahead and CTRL + . click and say `Generate method...`. Remove what's inside the new method, and replace it with:
```C#
using (var hmac = new System.Security.Cryptography.HMACSHA512())
{
  passwordSalt = hmac.Key;
  passwordHash = hmac.ComputeHash(System.Text.Encoding.UTF8.GetBytes(password));
}
```
