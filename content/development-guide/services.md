# Services and Dependency Injection

OpenMod, like other modern .NET projects, uses the dependency injection pattern. This guide aims to simplify it and explain what it means for plugin developers using OpenMod.

Plugins, commands, event listeners, and services can automatically get references to any other services provided by OpenMod or plugins just by adding their interfaces to the constructor.  

## Registering your own services
There are two ways to register a service:
1. Registering by using the`[Service]` attribute for the interface and `[ServiceImplementation]` for the concrete class.
2. Registering manually by implementing the `IServiceConfigurator` or `IContainerConfigurator` interfaces. Classes which implement these interfaces are automatically instantiated when the IoC container is configured and can be used to configure the container directly. 

## Built-in OpenMod services

| **Service**                                     |
|-------------------------------------------------|
| IConfiguration                                  |
| ICommandExecutor                                |
| ICommandStore                                   |
| ICommandPermissionBuilder                       |
| ICurrentCommandContextAccessor                  |
| IStringLocalizer                                |
| IOpenModStringLocalizer                         | 
| IDataStoreFactory                               | 
| IOpenModDataStoreAccessor                       | 
| IEventBus                                       | 
| IOpenModHost                                    | 
| IPermissionChecker                              | 
| IPermissionRolesDataStore                       |    
| IUserDataSeeder                                 | 
| IUserDataStore                                  | 
| IUserManager                                    | 
| IRuntime                                        | 


## Dependency injection example
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

## Further reading
For more, read the [Dependency injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection) page on docs.microsoft.com.
