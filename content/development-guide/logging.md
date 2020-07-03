# Logging

OpenMod uses the `Microsoft.Extensions.Logging` package for logging abstractions and [Serilog](https://serilog.net/) as the logging implementation for it.

For more, check out the [ILogger Interface documentation on docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-3.1). 

You can get a logger instance by injecting it via dependency injection:
```cs
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

The generic part (`XX` in `ILogger<XX>`) must equal to the class that is using the logger.