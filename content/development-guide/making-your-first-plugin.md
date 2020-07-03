# Making your first plugin

In this guide we will cover:

1. Setting up development environment.
2. Creating a plugin project.
3. Writing a basic plugin.

## Installing .NET Core SDK
Install the latest .NET Core SDK from [here](https://dotnet.microsoft.com/download/dotnet-core/3.1) (you should dowlnoad a dotnet-sdk-xxxxx.zip).

## Installing the IDE for coding
After setting up .NET Core SDK, we will need to install an IDE. The IDE provides us an environment where we can code our plugins.

### Visual Studio Code
You can use [install Visual Studio Code](https://code.visualstudio.com/) for developing plugins and is supported on Linux, MacOS and Windows. Visual Studio Code is the preferred solution for small- to mid-sized projects. It is supported for all OpenMod platforms.

### Visual Studio
If you want a full IDE experience, download and install [Visual Studio Community Edition](https://visualstudio.microsoft.com/vs/community/). Visual Studio is only supported on Windows platforms. When the installer starts, select "Visual Studio 2019 Community Edition" (or newer, if available). After that select the .NET Core cross-platform development and the .NET Desktop Development options like shown below. 

![Selecting .NET desktop development option](https://docs.microsoft.com/en-us/visualstudio/install/media/vs2017-modify-workloads.png?view=vs-2017g)

![Selecting .NET Core cross-platform development option](https://static.packt-cdn.com/products/9781787281905/graphics/image_05_002.png)

## Generating the Plugin Project
Start cmd or Powershell and navigate to the folder where you want to create the plugin project.

E.g.
```
mkdir C:\Users\<Username>\source\repos\MyPlugin\
cd C:\Users\<Username>\source\repos\MyPlugin\
``` 

After that, install the OpenMod Plugin Templates for .NET Core SDK:
```
dotnet new -i "OpenMod.Templates::*"
```

Finally, you can generate the plugin project with this command:  
```
dotnet new openmod-universal-plugin --PluginId <PluginId> [--FullPluginName <FullPluginName>]
```

or, if you want to develop a plugin for Unturned:  
```
dotnet new openmod-unturned-plugin --PluginId <PluginId> [--FullPluginName <FullPluginName>]
```

`PluginId` must be a unique identifier for your plugin. By convention, it is the same as the NuGet package ID.  
`FullPluginName` is optional and will set how your plugin should be displayed to the user.

To get the full help for the command, you can use the --help switch like this:  
```
dotnet new openmod-universal-plugin --help
``` 

or, for Unturned:  
```
dotnet new openmod-unturned-plugin --help
``` 

### Example
If you want to create an Unturned plugin project, you can use the following command:
```
dotnet new openmod-unturned-plugin --FullPluginName "My Unturned Plugin" --PluginId "MyName.MyUnturnedPlugin"
```

## Adding a basic Teleport Command
TODO

## Best Practices
Do not use static plugin instances, instead always pass instances by reference. The reason for that is that Rocket can dynamically create and destroy your plugin instances, which could result in wrong instances being used.