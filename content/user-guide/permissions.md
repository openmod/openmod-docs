# Permissions

OpenMod has a simple role-based permission system. Permissions define which actions a user can execute and which they cannot.

You can manage permissions by editing the `openmod.roles.yaml` file inside the `OpenMod` directory or by using the `permission` and `permissionrole` commands. You can use the `help permission` and `help permissionrole` commands for more information.

## Permission roles
Roles are basically a group of permissions and other attributes. If you assign a role to a user, they will automatically inherit all permissions of the role. You can also add parent roles to a role so they can inherit permissions.

A role has the following attributes:

- **Parents**: The parent roles, whose permissions are inherited.
- **Permissions**: List of permissions the role has.
- **Display Name**: Human-readable name of the role.
- **Is Auto Assigned**: Automatically assigns the role to new users. **Does not assign to existing users**.
- **Data**: Data that can be attached to the role by plugins. 
- **Priority**: In case of conflicting permissions, this attribute will define which role gets preferred.

### Creating permission roles
Open the `openmod.roles.yaml` file. You will see something similar to this:
```yaml
roles:
- id: default
  parents: []
  permissions:
  - help
  displayName: Default
  data: {}
  isAutoAssigned: true
- id: vip
  priority: 1
  parents:
  - default
  permissions:
  - home
  - tp  
  - kit.vip
  data: {}
```

This list contains 2 roles: `default` and `vip`. As you may have noticed, the `-` adds a new role.

To add a new role, simply copy the default role and add it like this:
```yaml
roles:
- id: default
  parents: []
  permissions:
  - help
  displayName: Default
  data: {}
  isAutoAssigned: true
- id: vip
  displayName: VIP
  priority: 1
  parents:
  - default
  permissions:
  - kit.vip
  - home
  - tp
  data: {}
- id: megavip
  displayName: Mega VIP
  priority: 1
  parents:
  - vip
  permissions:
  - kit.megavip
  data: {}  
```

!!! Note
    You can also add and remove parent roles with the `permissionrole` command: `permissionrole add role megavip vip`

### Removing roles
Just remove the role from the `openmod.roles.yaml` file.

### Managing role permissions
From the earlier example, the `megavip` role has the following permissions:
- help (inherited from default, which is a parent of vip)
- kit.vip (inherited from vip)
- kit.megavip

What if we want it to have `vip` as parent, but do not want it to inherit the `kit.vip` permission?  
In that case, we can negate the permission by adding it prefixed with a "!":

```
- id: megavip
  displayName: Mega VIP
  priority: 1
  parents:
  - default
  - vip
  permissions:
  - !kit.vip # Forcefully removes "kit.vip", even if inherited
  - kit.megavip
  data: {} 
```

!!! Note
    You can also add and remove permissions with the `permission` command: `permission add role megavip !kit.vip`

## User permissions and user roles
You can assign users to roles via the `permissionrole add player <player> <role>` command, e.g. `permissionrole add player Trojaner megavip`. The same way, you can remove users from a role with ``permissionrole remove player <player> <role>`

Users can also have permissions directly attached to them: `permission add player Trojaner kit.vip`. User permissions always override any conflicting role permissions. Use `permission remove` to remove the permission again.

## Permission wildcards
Assume a teleport plugin has the following permissions:

* teleport
* teleport.bring
* teleport.bring.request
* teleport.request

Instead of adding all of these one by one, you can use the * wildcard to add all of them:

* teleport.*

This will grant all permissions from above.

!!! Note
    Just adding the `teleport` permission will **not** grant the child permissions, e.g. teleport.bring.

## Finding command permissions
If you do not know what permission a command requires, you can use `help <command>` to find it. Permissions for subcommands are not granted automatically and must be given either by using wildcards on the parent command permission or by specifying them directly.