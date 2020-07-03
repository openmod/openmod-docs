# Services and Dependency Injection

##Introduction

OpenMod, like a lot of modern .NET projects, uses Dependency Injection. This guide aims to simplify it and explain what it means for plugin developers using OpenMod.

## What does Dependency Injection mean for me?

In short, you do not have to create public static instances of any of your classes.

Your plugins services can have a constructor with any services provided by OpenMod and these will all be passed automatically by the container.

You can also add your own services to the container, which will add them to the ecosystem. You do NOT need to do this for commands or event handlers however (they are registered automatically by OpenMod)

## List of services

TODOODODOODODODOODODODODOODODODO

## Example

I would like to access my plugin's configuration in a command. Here is how I do this:

```c#
//This is now accessible by the execute method
private IConfiguration m_Configuration;

public AnnounceCommand(IServiceProvider serviceProvider, IConfiguration configuration) : base(serviceProvider)
{
    m_Configuration = configuration;
}
```

## Further reading
****
If you'd like to know more about dependency injection and how it works - OpenMod is using the Autofac container. Look up examples of this on google.
