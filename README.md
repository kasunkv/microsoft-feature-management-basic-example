# Microsoft Feature Management Basic Example
Basic example of using Microsoft.FeatureManagement on ASP.Net Core Web Application

## Install required NuGet Packages
Install `Microsoft.FeatureManagement.AspNetCore` Nuget packag. Note that this package is in pre-release. 

## Configure required services in `Startup.cs`

Make the following changes in the `Startup.cs` file, Under `ConfigureServices()` method

```csharp

public void ConfigureServices(IServiceCollection services)
{
    ...

    // Configure Feature Management
    services.AddFeatureManagement();

    ...
}
```

## Add tag helpers to `_ViewImports.cs` file
If you want to use the tag helpers provided, you need to add the following line into _ViewImports.cshtml file.

```asp
@addTagHelper *, Microsoft.FeatureManagement.AspNetCore
```

## use `feature` tag helper to selectively show HTML content

```html
<feature name="@Features.Promotions">
    ...
</feature>
```

## Inject `FeatureManager` to your Controllers, Services classes
After injecting `FeatureManager` to your classes you can use it to check if features are enabled or disabled.

```csharp
public HomeController(IAlbumService albumService, IFeatureManagerSnapshot featureManager)
{
    _albumService = albumService;
    _featureManager = featureManager;
}
```

## Use `IsEnabledAsync()` to check for feature enabled/disabled state.
In your C# code you can use `IsEnabledAsync() to selectively enable changes

```csharp
public async Task<IActionResult> Index()
{
    ...

    if (await _featureManager.IsEnabledAsync(Features.UserSuggestions))
    {
        // Do things when UserSuggestion feature is enabled
    }

    ...
}
```

## Use `FeatureGate` attribute to enable/disable Controller Actions
```csharp
[FeatureGate(Features.Promotions)]
public async Task<IActionResult> Promotions()
{
    ...
}
```