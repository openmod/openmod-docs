# Publishing to nuget.org
If you want your plugin to be installable from `openmod install`, you need to publish it to nuget.org.
You will need a Microsoft account for [nuget.org](https://www.nuget.org/). 

# Preparing your plugin for NuGet
1. Add the following to your plugin's .csproj
```xml
<PropertyGroup>
  <PackageId>Your PackageId</PackageId> <!-- must be unique, should be same as your plugin ID -->
  <PackageDescription>Your plugin description</PackageDescription>
  <PackageLicenseExpression>Your License</PackageLicenseExpression> <!-- see https://spdx.org/licenses/ -->
  <PackageAuthor>Your name</PackageAuthor>
  <PackageTags>openmod openmod-plugin XXX</PackageTags> <!-- XXX can be unturned, unityengine or universal depending on your plugin -->
  <Version>x.x.x</Version> <!-- Your plugins version. Must be semversion, see https://semver.org/ -->
  <AssemblyVersion>x.x.x</AssemblyVersion> <!-- set same as package version, required for dynamicalliy updating your plugin -->
  <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
  <GenerateNugetPackage>true</GenerateNugetPackage>  
</PropertyGroup>
```
2. Sign in to [nuget.org](https://nuget.org).
3. Click on your username, select API Keys.
4. Select create. Add a name, select the `Push` scope and add `*` as Glob pattern, then select create.
5. Copy your newly created key. Save your key securely as you can not retrieve it again.
6. Save your key: 

## Uploading the plugin
1. Navigate to your plugin's folder. Execute the following command: `dotnet build --configuration Release`.
2. Go to `bin/Release/` and push to NuGet: `dotnet push <yourpackageid.x.x.x.nupkg> -n -k <your nuget.org key> -s https://api.nuget.org/v3/index.json`
    
You can install your plugin with `openmod install <YourPackageId>`.

!!! Note
    You may get the "PackageOrVersionNotFound" error after trying to install your plugin. This means your upload has not 
    been verified or indexed yet. It usually takes about an hour until nuget.org uploads are verified and indexed.

For more, read the [Publish to nuget.org](https://docs.microsoft.com/en-us/nuget/nuget-org/publish-a-package) and [Create and publish a NuGet package (dotnet CLI)](https://docs.microsoft.com/en-us/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli) pages on docs.microsoft.com.