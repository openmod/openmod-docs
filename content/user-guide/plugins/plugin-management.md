# Installing and Managing Plugins
OpenMod provides commands to download, install, update and remove plugins at runtime.

!!! Note
    You must restart the server or reload OpenMod with `openmod reload` after doing any changes to installed plugins.

!!! Note
    For more help, you can use the `help openmod` command.

## Finding Plugins
You can find a list of plugins at [the openmod-plugins repository](https://github.com/openmod/openmod-plugins). You can also search for openmod-plugin on [nuget.org](https://nuget.org).

## Installing Plugins
There are two ways to install plugins by default:

1. To install plugins from NuGet, install them via the `openmod install <package id>` command, e.g. `openmod install OpenMod.Rocket.Unturned`.
   You can also install specific versions via the `openmod install <package id> <version>` command.
   For pre release versions, add the `-Pre` option: `openmod install <package id> -Pre` or `openmod install <package id> <version> -Pre`.
2. To install plugins manually, move the plugin dll file and all libraries of the plugin to the `openmod/plugins` directory. You can also install the libraries with `openmod install <package-id>` instead.

# Updating Plugins
If you installed the plugin via `openmod install`, you can simply update it using `openmod update <package id>`.  
Like with `openmod install`, you can specify the version and use `-Pre` for pre release versions.

If you installed the plugin directly as dll file, you can delete replace the old .dll file with the updated one.

## Removing Plugins
If you installed the plugin via `openmod install`, you can simply remove it by using `openmod remove <package id>`.  

If you installed the plugin directly as dll file, you can just delete the .dll file.