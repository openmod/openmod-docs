# Permissions
Permissions allow defining which actions a user is permitted to execute and which he is not.

You can use the `IPermissionChecker` service to check if a user has specific permissions:

```c#
// read event documentation for more information about event listeners
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

        if(m_PermissionChecker.CheckPermissionAsync(user, "announce.join") == PermissionGrantResult.Grant)
        {
            await m_UserManager.BroadcastAsync(user.Type, $"{user.DisplayName} has joined.");
        }
    }
}
```

Let's have a closer look at `CheckPermissionAsync`.
`CheckPermissionAsync` returns `PermissionGrantResult`, which is an enum with members:

* Default - The permission is neither explicitly granted nor explicitly denied
* Grant - The permission was explicitly granted
* Deny - The permission was explicitly denied

Usually, you want to check if the result equals to `PermissionGrantResult.Grant` to permit an action. This means that if no explicit permission is set, the action will be denied by default. If you want to execute an action unless it is explicitly denied, use `CheckPermissionAsync(..) != PermissionGrantResult.Deny`.

## Adding your own permissions
You can add your own permissions (e.g. to store permissions in MySQL):

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

## Override permission checks
Sometimes you may want to check yourself if an actor has a permission.

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

The following example will grant all permissions to Unturned admins:
```c#
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