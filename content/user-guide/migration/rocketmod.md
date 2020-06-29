# Migrating from RocketMod to OpenMod

OpenMod has built-in support for RocketMod plugins, most RocketMod plugins should work without need for workarounds or rewrites. 

To use use RocketMod plugins, follow these steps:
1. After [installing OpenMod for Unturned](../installation/unturned.md) remove the "Rocket.Unturned" folder from the "Modules" folder inside Unturned. If you were using RocketMod before, keep the "Rocket" folder inside your server's folder.
2. Start your server, wait for OpenMod to load and then install the [OpenMod RocketMod Bridge](https://github.com/openmod/openmod/tree/master/unturned/rocketmod): `openmod install OpenMod.Rocket.Unturned`.
3. To finish migration, restart your server or reload OpenMod: `openmod reload`.

You can install and use RocketMod plugins just like how you would do with the RocketMod module: add the dll files to your "Rocket/Plugins" folder inside your server's folder. RocketMod is completely separated from OpenMod and has it's own configuration system, permissions system, command system etc. The RocketMod plugins are *not* converted to OpenMod plugins. OpenMod commands will always override RocketMod commands in case of conflicting commands.

## Using RocketMod with OpenMod Permissions
By default, the OpenMod RocketMod Bridge will use RocketMod's own Permissions.xml when handling permissions for RocketMod plugins. This might become a problem as you would have to maintain two different permission systems. For example, if you wanted to add a new group or permission, you would have to edit OpenMod's permission file and RocketMod's permission file to synchronize them. The PermissionLink solves this issue by forcing RocketMod to use OpenMod's permission system. Keep in mind that this solution may not be compatible with other permissions plugins for RocketMod.

To install the permission link, run `openmod install OpenMod.Rocket.PermissionLink` and then reload OpenMod or restart.

## Differences to Original RocketMod
* The OpenMod RocketMod Bridge is based on LDM RocketMod's source code, it is not a new implementation of the RocketMod API. This will ensure compatibility with plugins which access private fields via reflection.
* RocketMod is now an OpenMod plugin instead of an Unturned module.
* The original RocketMod does not support unloading at runtime, as it never unbindings from events. With OpenMod, you can completely unload RocketMod or OpenMod at runtime.
* RocketMod command events have been changed to utilize OpenMod's command system.
* RocketMod logging system has been changed to proxy to OpenMod's logging system (Serilog).