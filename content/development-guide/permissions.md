# Permissions
Permissions allow to define which action a user is allowed to execute and which he is not.

You can use the `IPermissionChecker` service to check if a user has specific permissions:

```c#
// read event documentation  for more information about event listeners
public class UserConnectBroadcaster : IEventListener<UserConnectedEvent> 
{
    private readonly IPermissionChecker m_PermissionChecker;
    private readonly IUserManager m_UserManager;

    public UserConnectEventListener(
        IPermissionChecker permissionChecker,
        IUserManager userManager)
    {
        m_PermissionChecker = permissionChecker;
        m_UserManager = userManager;
    }

    public async Task HandleEventAsync(object sender, UserConnectedEvent @event)
    {
        var user = @event.User;

        if(m_PermissionChecker.CheckPermissionAsync(user, "announce.join") != PermissionGrantResult.Deny)
        {
            await m_UserManager.BroadcastAsync(user.Type, $"{user.DisplayName} has joined.");
        }
    }
}
```

Let's have a closer look at `CheckPermissionAsync`.
`CheckPermissionAsync` returns `PermissionGrantResult`, which is a enum with members:

* Default - The permission is neither explicitly granted nor explicitly denied
* Grant - The permission was explicitly granted
* Deny - The permission was explicitly denied

In the most cases you would check if the result equals `PermissionGrantResult.Grant`. This means that if no explicit permission is set, the action will be denied by default. In the example above the opposite has been done: unless the permission was explicitly denied the join message will be announced. 

## Adding your own permissions
You can add your own permissions (e.g. to support permissions stored in MySQL):

1. Implement IPermissionStore
2. Add your IPermissionStore via a [ServiceConfigurator](../services.md#registering-your-own-services):
```c#
public class ServiceConfigurator : IServiceConfigurator
{
    public void ConfigureServices(IOpenModStartupContext openModStartupContext, IServiceCollection serviceCollection)
    {
        serviceCollection.Configure<PermissionCheckerOptions>(options =>
        {
            options.AddPermissionSource<YourPermissionStore>();
        });        
    }
}
```

## Checking if an actor has a certain permission
Sometimes you may want to check yourself if an actor has a permission. In that case do the following:

1. Implement IPermissionCheckSource
2. Add your IPermissionCheckSource via a [ServiceConfigurator](../services.md#registering-your-own-services):
```c#
public class ServiceConfigurator : IServiceConfigurator
{
    public void ConfigureServices(IOpenModStartupContext openModStartupContext, IServiceCollection serviceCollection)
    {
        serviceCollection.Configure<PermissionCheckerOptions>(options =>
        {
            options.AddPermissionCheckSource<YourPermissionCheckSource>();
        });        
    }
}
```

The following example grants all permissions to all Unturned admins:
```cs
public class UnturnedAdminPermissionCheckProvider : IPermissionCheckProvider
{
    public bool SupportsActor(IPermissionActor actor)
    {
        /* only apply to unturned admins */
        return actor is UnturnedUser user && user.SteamPlayer.isAdmin;
    }

    public Task<PermissionGrantResult> CheckPermissionAsync(IPermissionActor actor, string permission)
    {
        /* grant on any permission */
        return Task.FromResult(PermissionGrantResult.Grant);
    }
}
```