# Preparing to develop

## Required software

* .NET Core sdk

## IDEs

IDEs (integrated development environment) are always good for a projects like these.

* Jetbrains Rider - Can be acquired for free with a student license. Works on Linux

* Visual Studio - Community or Enterprise editions

* Visual Studio with Omnisharp extension (more lightweight than VS)


# Preparing the project for development

Create a new solution and a .net standard 2.1 project, call it something sensical like the name of your plugin. Open the NuGet package manager located in your IDE and install the following packages:

* OpenMod.Core

* OpenMod.API

The following are dependant on what kind of plugin you want to make:

* OpenMod.Unturned

* OpenMod.UnityEngine


If you're not developing a univerval plugin, make sure to include the base game assemblies (with unity they tend to be called Assembly-CSharp.dll)



