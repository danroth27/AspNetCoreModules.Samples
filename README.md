# ASP.NET Core Modules Samples

## Setup

- Install [.NET Core 1.1 Preview 1](https://github.com/dotnet/core/blob/master/release-notes/preview-download.md).
- Clone the ASP.NET Core Modules Samples repo
- Restore and Run

## Manual setup

You can setup ASP.NET Core modules manually for an ASP.NET Core 1.1 Preview 1 app by doing the following:

- Add the [Microsoft.AspNetCore.Modules](https://www.myget.org/feed/aspnetcoremodules/package/nuget/Microsoft.AspNetCore.Modules) package from the [aspnetcoremodules](https://www.myget.org/feed/Packages/aspnetcoremodules) MyGet feed.
- Modify your Startup class to call `services.AddModules()` in your `ConfigureServices` method and `app.UseModules()` from your `Configure` method.
- Add module packages (ex [Module1](https://www.myget.org/feed/aspnetcoremodules/package/nuget/Module1) or [Microsoft.AspNetCore.Identity.Module](https://www.myget.org/feed/aspnetcoremodules/package/nuget/Microsoft.AspNetCore.Modules)) to your app
 
### Module1 specific steps

- Module1 uses the default route. If your app also uses the default route then you will probably need to specify a path base for Module1. You can do this using the `ModuleOptions.PathBase` property in your `services.AddModule()` method call, like this:

  ```c#
  services.AddModules(options => options.PathBase["Module1"] = "/Module1");
  ```

- Module1 has a HomeController. If your app also has a HomeController you will want to configure the main app to ignore controllers in modules by calling `services.AddMvc().IgnoreModules()`.

### Identity module specific steps

- The Identity module needs a connection string for the user data base. Add a connection string to your `appsettings.json` file like this:

  ```json
  {
    "ConnectionStrings": {
      "DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=aspnet-IdentityModule-B374662C-FADA-4E82-96E9-B7042CDC6E02;Trusted_Connection=True;MultipleActiveResultSets=true"
    }
  }
  ```
 
- Configure the connection string for the Identity module by calling `services.Configure<IdentityModuleOptions>()` before calling `servcies.AddModules()` like this:
 
  ```c#
  services.Configure<IdentityModuleOptions>(options => options.ConnectionString = Configuration.GetConnectionString("DefaultConnection"));
  services.AddModules();
  ```

- Use the `IdentityLoginPartialAsync()` HTML helper to add the login partial to your app layout page. You'll need to add a using statement for the `Microsoft.AspNetCore.Identity.Module` namespace.

  ```c#
  @using Microsoft.AspNetCore.Identity.Module
 
  @await Html.IdentityLoginPartialAsync()
  ```
