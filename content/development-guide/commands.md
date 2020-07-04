# Commands
OpenMod provides a strong command framework.

To create a command, simply create a class that inherits from one of these:

* Command (for universal plugins)
* UnityEngineCommand (for UnityEngine plugins)
* UnturnedCommand (for Unturned plugins)

In this example, we will make a universal command:
```c#
public class CommandAwesome : Command
{
    public CommandAwesome(IServiceProvider serviceProvider) : base(serviceProvider)
    {
    }
}
```

!!! Note
    In the following examples no translations are used. See [translations](../translations.md) on information on 
    how to integrate translation files.

After that, add some metadata to describe our command and its usage:
```c#
[Command("awesome")] // The primary name for the command. Usually, it is defined as lowercase. 
[CommandAlias("awsm")] // Add "awsm" as alias.
[CommandAlias("aw")] // Add "aw" as alias.
[CommandDescription("My awesome command")] // Description. Try to keep it short and simple.
public class CommandAwesome : Command
{
    public CommandAwesome(IServiceProvider serviceProvider) : base(serviceProvider)
    {
    }
}
```

Finally implement OnExecuteAsync:
```c#
[Command("awesome")] 
[CommandAlias("awsm")]
[CommandAlias("aw")]
[CommandDescription("My awesome command")]
public class CommandAwesome : Command
{
    public CommandAwesome(IServiceProvider serviceProvider) : base(serviceProvider)
    {
    }

    public async Task OnExecuteAsync() // use UniTask instead of Task if derivered from UnityEngineCommand or UnturnedCommand
    {
        var actor = Context.Actor;
        await actor.PrintMessageAsync("You are awesome!");
        // await PrintAsync("You are awesome"); // alternatively, you can use this shortcut.
    }
}
```

## Parameters
When we handle commands, we usually also need to handle parameters. The command context provides a Parameter property. Let's use it:  
```c#
public async Task OnExecuteAsync()
{
    // assume we want the command to be called like this: /awesome <player> <amount>
    // Parameters start from 0, so <player> index is 0, <amount> index is 1.
    var player = Context.Parameters.Get<string>(0);
    var count = Context.Parameters.Get<int>(1);
    await PrintAsync($"{player} is {amount}x awesome!");
}
```

After that, we need to describe how users can use the command. Add the syntax metadata to the command class:
```c#
[CommandSyntax("<player> [amount]")] // Describe the syntax/usage. Use <> for required arguments and [] for optional arguments.
```

## Restricting Command Actors
If you are not developing universal plugins, you may want to limit who can execute commands.  
The `[CommandActor(Type)]` attribute allows you to specify such restrictions.

For example, if you would like to restrict a command's usage to UnturnedUser and ConsoleUser, you could add the following:
```c#
[CommandActor(typeof(UnturnedUser))]
[CommandActor(typeof(ConsoleUser))]
```

## Exceptions
Exceptions derived from `UserFriendlyException` are automatically caught by OpenMod and displayed to the user in a user friendly way.  

These built-in exceptions available: 

* NotEnoughPermissionException, can be thrown if the user does not have enough permission to execute an action.
* CommandWrongUsageException, can be thrown on wrong command usage. Displays correct usage based on command syntax.
* CommandIndexOutOfRangeException, thrown automatically by Parameters.Get if the given index is bigger than the arguments length.
* CommandParameterParseException, thrown automatically by Parameters.Get if the parameter could not be parsed to the expected type.
* CommandNotFoundException, thrown automatically if a command was not found.

```c#
public async Task OnExecuteAsync()
{
    var player = Context.Parameters.Get<string>(0);
    var count = Context.Parameters.Get<int>(1);

    if(count < 1) 
    {
        throw new UserFriendlyException("Count can not be negative!");
    }

    await PrintAsync($"{player} is {amount}x awesome!");
}
```

## Command Permissions
By design and for consistency reasons, you can not define a command permission manually. OpenMod will automatically assign a permission to the command instead. You can use the `help <yourcommand>` command to figure out what the base permission for your command is.

Assume you want to restrict the `awesome` command if count is more than 10. This is how you would do it:
```c#
public async Task OnExecuteAsync()
{
    var player = Context.Parameters.Get<string>(0);
    var count = Context.Parameters.Get<int>(1);

    if(count > 10 && await CheckPermissionAsync("MoreThan10") != PermissionGrantResult.Grant) 
    {
        throw new NotEnoughPermissionException(this, "MoreThan10"); // displays an error message to the user 
    }

    await PrintAsync($"{player} is {amount}x awesome!");
}
```

## Adding Subcommands
You can add subcommands to a command by using the `[CommandParent]` attribute. This allows OpenMod to discover your subcommands and provide additional help and tab autocompletion.

The following command will execute when a user types "/awesome more". The `CommandAwesome.OnExecuteAsync` method will not execute in this case.
```c#
[Command("more")] 
[CommandDescription("My more awesome command")]
[CommandParent(typeof(CommandAwesome))] // set "awesome" as parent.
public class CommandAwesomeMore : Command
{
    public CommandAwesomeMore(IServiceProvider serviceProvider) : base(serviceProvider)
    {
    }

    public async Task OnExecuteAsync()
    {
        // Note: Parameters do not include "more".
        // If you type "/awesome more a", Context.Parameters[0] will be equal to "a". 
        await PrintAsync("You are even more awesome!");
    }
}
```

## Best Practices
* **Do not** handle sub commands yourself (e.g. `if(Context.Parameters[0] == "add")`). OpenMod can not discover your subcommands and provide additional help or tab completion in that case.  
* **Do not** hardcode messages. Instead, use [translations](../translations.md) so users can customize and translate your messages.
* When writing commands, keep in mind that any type of user could execute your command by default. Maybe a plugin adds a DiscordUser and someone from Discord executes your command. Try to write your commands in a way that works with all kinds of users or restrict the allowed actors as mentioned earlier.
* **Do not** manually check if an actor is allowed to execute a command (e.g. `if(!(actor is UnturnedUser))`). Always use `[CommandActor]` for such restrictions. It will automatically hide the command from actors who can not execute them and give a consistent error message. 