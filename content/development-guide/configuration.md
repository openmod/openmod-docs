# Configurations
Configurations allow users to configure and customize your plugin's behavior.  
Assume your plugin gives XP when killing a player. By using configs, a user can dynamically set how much XP will be given. 

OpenMod configurations are based on [Microsoft.Extensions.Configuration](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.configuration?view=dotnet-plat-ext-3.1), which are also used in ASP.NET Core.

!!! Note
    The **\<RootNamespace\>** and **\<AssemblyName\>** properties in the plugin's .csproj file must be equal, otherwise the `IConfiguration` service will not work.

## Adding the config.yaml
Create a new file called "config.yaml" inside your project's root directory.  
After that, add the following to your .csproj file: 
```xml
<ItemGroup>
  <EmbeddedResource Include="config.yaml" />
</ItemGroup>
```

## Reading from the configuration file
You can use the `IConfiguration` service to read the config.yaml file. You can inject it like this:  
```c#
public class MyPlugin : OpenModUniversalPlugin
{
    private readonly IConfiguration m_Configuration;

    public MyPlugin(IConfiguration configuration, IServiceProvider serviceProvider) : base(serviceProvider)
    {
        m_Configuration = configuration;
    }

    public async Task OnLoadAsync()
    {
        // ...
    }
}
```

Assume your configuration looks like this:
```yaml
plugin_load_delay: 500
```

Then this is how you would read the value:
```c#
public async Task OnLoadAsync()
{
  var delay = m_Configuration.GetSection("plugin_load_delay").Get<int>();
  await Task.Delay(delay);
}
```

You can also have nested values:
```yaml
plugin_load:
  actions:
    wait_delay: 500
```

```c#
public async Task OnLoadAsync()
{
  // notice how ":" is used to access nested values
  var delay = m_Configuration.GetSection("plugin_load:actions:wait_delay").Get<int>();
  await Task.Delay(delay);
}
```

If you want to access strings, you can also use the indexer:
```yaml
players:
  owner: "Trojaner"
```
```c#
public async Task OnLoadAsync()
{
  // reading strings is even easier
  string owner = m_Configuration["players:owner"];
}
```

!!! Note
    Configurations are not meant to act as a storage for data, that's why there is no save method. Use [Data Stores](../datastore/) if you have read from and write data to a file.

## Converting configuration to C# class
```c#
public async Task OnLoadAsync()
{
  MyConfigClass config = m_Configuration.Get<MyConfigClass>();
  // or if you only want a subset:
  // MyClass config = m_Configuration.GetSection("something").Get<MyClass>();
}
```

## Adding your own configuration sources
You can add additional configuration sources by implementing the `IConfigurationConfigurator` interface.  
This interface allows you to add additional sources to the `IConfigurationBuilder` used when building the OpenMod config.

!!! Note
    Custom configuration sources for plugins [are not supported yet](https://github.com/openmod/openmod/issues/90).

## Best Practices
* **Do not** hardcode important values. For example, if you are making a plugin that gives players XP when killing other players, make sure the XP amount is configurable, as it is likely something a user would want to adjust.
* **Do not** overcomplicate your configurations. Only add values that users are likely going to change. Have a simple configuration is preferred to a complex one.
* **Do not** use configurations to store messages. Use [translations](../localization/) instead. Unlike translations, configurations do not support any kind of formatting or passing arguments.
