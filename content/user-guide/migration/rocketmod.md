# Migrating from RocketMod to OpenMod

OpenMod has built-in support for RocketMod plugins, most RocketMod plugins should work without need for workarounds or rewrites. 

To use use RocketMod plugins, follow these steps:

1. After [installing OpenMod for Unturned](../installation/unturned.md) remove the "Rocket.Unturned" folder from the "Modules" folder inside Unturned. If you were using RocketMod before, keep the "Rocket" folder inside your server's folder.
2. Start your server, wait for OpenMod to load and then install the [OpenMod RocketMod Bridge](https://github.com/openmod/openmod/tree/master/unturned/rocketmod): `openmod install OpenMod.Rocket.Unturned`.
3. To finish migration, restart your server or reload OpenMod: `openmod reload`.

## Using the OpenMod RocketMod Bridge
You can use the OpenMod RocketMod Bridge exactly like the RocketMod module.  
For example, to install RocketMod plugins just add the dll files to your `Rocket/Plugins` folder inside your server's folder and restart or reload RocketMod: `rocketmod reload`.

## Caveats
- RocketMod plugins are *not* converted to OpenMod plugins, so you can not manage them from OpenMod.
- RocketMod is completely separated from OpenMod and has it's own configuration system, permissions system, command system etc. 
- OpenMod commands will always override RocketMod commands when a conflict occurs.

## Linking RocketMod to OpenMod Permissions
By default, the OpenMod RocketMod Bridge will use RocketMod's own Permissions.xml when handling permissions for RocketMod plugins. This might be a problem as you would have to maintain two different permission systems. The RocketMod Permission Link plugin solves this issue by forcing RocketMod to use OpenMod's permission system. Keep in mind that it may not be compatible with other permissions plugins for RocketMod.

To install the RocketMod Permission Link plugin, run `openmod install OpenMod.Rocket.PermissionLink` and then reload or restart.

## Differences between the OpenMod RocketMod Bridge and RocketMod
- The OpenMod RocketMod Bridge is based on a patched version of the [LDM RocketMod fork](https://github.com/SmartlyDressedGames/Legally-Distinct-Missile), it is not a reimplementation of the RocketMod API. This will ensure compatibility with most RocketMod plugins, including those which access private RocketMod fields via reflection.
- The OpenMod RocketMod Bridge is an OpenMod plugin instead of a standalone Unturned module.
- RocketMod does not support unloading at runtime, as it never unbinds from events. With OpenMod, you can completely unload RocketMod or OpenMod at runtime.
- RocketMod's command handling has been changed to utilize OpenMod's command system to avoid conflicting commands.
- RocketMod's logging system has been changed to proxy to OpenMod's logging system (Serilog).