# Installing OpenMod for Unturned

1. If you are running on a Linux system, install the `libgdiplus` package.
2. Download the latest OpenMod.Unturned.Module-vX.X.X.zip from [here](https://github.com/openmod/OpenMod/releases/latest).
3. Copy the "OpenMod.Unturned" folder into the "Modules" folder inside the Unturned installation directory.
4. Start your server. The first start will take a while since OpenMod will download its core components.
5. OpenMod supports RocketMod plugins. Read the [RocketMod Migration Guide](../migration/rocketmod.md) if you are migrating from RocketMod.
6. Done! Now you can [start installing plugins](../plugins/plugin-management.md).


## Using OpenMod on multiple servers sharing the same Unturned instance
If you want to use OpenMod for multiple Unturned servers sharing the same Unturned instance, you must adjust the `logging.yaml` file.

By default, Serilog logs to the `logs/openmod.log` file:
```yaml
- Name: File
  Args:
    path: logs/openmod.log
```

This works great if you use only one server per Unturned instance. If you have multiple servers sharing the same instance, this would result in all servers sharing the same log file. To fix this issue, adjust the value to include the servers name. E.g.:
```yaml
- Name: File
  Args:
    path: logs/openmod-myserver.log
```

You must repeat this for each server.