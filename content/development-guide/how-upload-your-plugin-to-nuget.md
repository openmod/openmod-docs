# How upload your plugin to Nuget.org

For this guide you need:

1. A `.nupkg` from your plugin.
2. A microsoft account to use [Nuget.org](https://www.nuget.org/). 

## How get the `.nupkg` from your plugin
Go to **YourPluginFolder/bin/Debug** and get the `.nupkg` file.

## Uploading the plugin to Nuget
First go to [Nuget.org](https://www.nuget.org/) and if you not already do, sign in with your microsoft account later select **Upload** on the top menu.
You will see something like this:

![Nuget Upload Site](https://docs.microsoft.com/en-us/nuget/nuget-org/media/publish_uploadyourpackage.png)

Now select **Browse** and upload your `.nupkg` file to the site, nuget.org tells you if the package name is available.

!!! Note
    If your package name isn't available you can change this in the plugin metadata above the namespace in your main **.cs**
    
If the package name is available, nuget.org opens a Verify section in which you can review the metadata from the package manifest.
With the **PackageId** you can install your plugin in your server using `openmod install {PackageId}`.

Under all information you can see a section to add documentation for your plugin.

When all the information is ready, select the **Submit** button.
