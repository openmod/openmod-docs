# Services and Dependency Injection

OpenMod, like other modern .NET projects, uses the [dependency injection pattern](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection). This guide aims to simplify it and explain what it means for plugin developers using OpenMod.

Plugins, commands, event listeners, and services can automatically get references to plugins or any other services provided by OpenMod just by adding their interfaces to the constructor. 


## Dependency injection example
Let's see this in action:

This is how your plugin looks like normally:
```c#
public class MyPlugin(IServiceProvider serviceProvider) : base(serviceProvider)
{

}
```

If you wanted to access the `IStringLocalizer` service, you could it like this:
```c#
public class MyPlugin(IStringLocalizer stringLocalizer, IServiceProvider serviceProvider) : base(serviceProvider)
{
   // do something with stringLocalizer
}
```

Assume you want to access your plugin's instance and configuration from a command. Here is how you could do it:
```c#
private readonly IConfiguration m_Configuration;
private readonly MyPlugin m_MyPlugin;

public EchoCommand(
    IServiceProvider serviceProvider, 
    MyPlugin myPlugin,
    IConfiguration configuration) : base(serviceProvider)
    m_MyPlugin = myPlugin;
    m_Configuration = configuration;
}
```

## Registering your own services
There are two ways to register a service:

1. Registering by using the`[Service]` attribute for the interface and `[ServiceImplementation]` for the concrete class.
2. Registering manually by implementing the `IServiceConfigurator` or `IContainerConfigurator` interfaces. Classes which implement these interfaces are automatically instantiated when the IoC container is configured and can be used to configure the container directly. 

You can implement the `IDisposable` or the `IAsyncDisposable` interface for cleaning up resources when OpenMod reloads or shuts down. 

You can use the `IPluginAccessor<>` service, to access your plugins instance and its services. 

Here is an example service to clear vehicles:
```c#
[Service]
public interface IVehicleClearingService
{
    Task ClearVehiclesAsync();
}

[ServiceImplementation(Lifetime = ServiceLifetime.Transient)]
public class VehicleClearingService : IVehicleClearingService, IAsyncDisposable
{
    private readonly IStringLocalizer m_StringLocalizer;
    private readonly ILogger<VehicleClearingService> m_Logger;
    public VehicleClearingService(
        ILogger<VehicleClearingService> logger, 
        IPluginAccessor<VehicleClearerPlugin> pluginAccessor)
    {
        VehicleClearerPlugin plugin = pluginAccessor.Instance;

        // Services live in the global OpenMod scope, which does not provide a IStringLocalizer.
        // Since IStringLocalizer does not exist in this scope, we have to use the plugins scope.
        m_StringLocalizer = plugin.Lifetime.Resolve<IStringLocalizer>();
        m_Logger = logger;
    }

    public async Task ClearVehiclesAsync() 
    {
        m_Logger.LogInformation(m_StringLocalizer["messages:clearing_vehicles"]); // translation is read from the plugins translation
        // call game apis to clear vehicles...
    }

    // Service dispose methods are called on OpenMod reload or server shutdown 
    public async ValueTask DisposeAsync()
    {
        await ClearVehiclesAsync(); // ensure vehicles get cleared on reload or shutdown
    }
}
```

You can now access this service by injecting `IVehicleClearingService` in e.g. commands, event listeners, your plugin class or other services. 

!!! Note
    Custom services have a different lifetime then plugins. Even if your plugin unloads your service will be still alive and used.    
    Services are created before plugins load and are destroyed when openmod reloads or the server shuts down.

### Service Lifetimes
You might have noticed that the VehicleClearingService's Lifetime was set to `Lifetime = ServiceLifetime.Transient`.  

The following lifetimes are available:

* **Transient**: The service gets recreated every time it gets resolved. Every resolution and injection of this service will have a unique instance. This is the default lifetime.
* **Scoped**: The service will share the same instance across the same command or event.
* **Singleton**: The service will only have one instance that lives as long as the OpenMod IoC container does (until OpenMod gets reloaded or the server shuts down).  

## Built-in OpenMod services

| **Service**                                     | **Description**                                   |
|-------------------------------------------------|---------------------------------------------------|
| IConfiguration                                  | Read configuration files                          |
| ICommandExecutor                                | Execute command                                   |
| ICommandStore                                   | Access command registrations                      |
| ICommandPermissionBuilder                       | Get a commands permission                         |
| ICurrentCommandContextAccessor                  | Access the current command context                |
| IDataStore                                      | Serialize and deserialize persistent data         |
| IDataStoreFactory                               | Create a data store instance                      |
| IEventBus                                       | Subscribe to events and emit them                 |
| IOpenModStringLocalizer                         | Localize messages from OpenMod's translation file |
| IOpenModDataStoreAccessor                       | Access OpenMods own data store                    |
| IOpenModHost                                    | OpenMod host platform abstractions                | 
| IPermissionChecker                              | Check permissions                                 |
| IPermissionRoleStore                            | Get and manage permission roles                   |
| IPluginAccessor<>                               | Access a plugins instace                          |
| IPluginActivator                                | Load and activate plugins                         |
| IRuntime                                        | Manage OpenMod runtime                            |
| IStringLocalizer                                | Localize messages from plugin translation files   |
| IUserDataSeeder                                 | Seeds user data on first connect                  |
| IUserDataStore                                  | Reads and saves user data                         |
| IUserManager                                    | Finds and gets users                              |

## Best Practices
* **Avoid** making your services singleton unneccessary. This may cause problems if your service has transient or scoped dependencies.