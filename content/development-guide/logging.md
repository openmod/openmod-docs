# Logging

OpenMod uses the `Microsoft.Extensions.Logging` package for logging abstractions and [Serilog](https://serilog.net/) as the logging implementation for it.

For more, check out the [ILogger Interface documentation on docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-3.1). 

You can get a logger instance by injecting it via dependency injection:
```c#
public class MyPlugin : OpenModUniversalPlugin
{
    private readonly ILogger<MyPlugin> m_Logger; 

    public MyPlugin(ILogger<MyPlugin> logger, IServiceProvider serviceProvider) : base(serviceProvider)
    {
        m_Logger = logger;
        m_Logger.LogInformation("Hello world!");
    }
}
```

The generic part (`XX` in `ILogger<XX>`) must be the class that is using the logger.

## Implementing your own logger
To implement your own logger, you must implement the `ILoggerFactory`, `ILogger` and `ILogger<>` services.  
After that you must register them via a [ServiceConfigurator](../services.md#registering-your-own-services):
```c#
public class ServiceConfigurator : IServiceConfigurator
{
    public void ConfigureServices(IOpenModStartupContext openModStartupContext, IServiceCollection serviceCollection)
    {
        serviceCollection.AddSingleton<ILoggerFactory, MyLoggerFactory>();
        serviceCollection.AddTransient<ILogger, MyLogger>(); // must be transient
        serviceCollection.AddTransient(typeof(ILogger<>), typeof(MyLogger<>)(); // must be transient
    }
}
```

!!! Note
    Your custom logger will not be used while OpenMod is booting and creating the IoC container. It will used once OpenMod has fully started.

## Best Practices
* **Do not** inject the `ILogger` interface directly. This will prevent the logger from associating your log messages with your class  your plugin. A user can also not configure minimum log levels or other options for your log messages in that case.