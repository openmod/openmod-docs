# Localization
Localization allows users to customize and translate your plugins messages.  
This is achieved through the translations.yaml and the IStringLocalizer service.  

## Adding the translations.yaml
Create a new file called "translations.yaml" inside your projects root directory.  
After that, add the following to your .csproj file: 
```xml
<ItemGroup>
  <EmbeddedResource Include="translations.yaml" />
</ItemGroup>
```

!!! Note
    If you used one of the templates to create your plugin project, the translations.yaml file will be already set up.

## Using IStringLocalizer for localization
Assume you want to localize the following command:
```c#
[Command("awesome")]
public class AwesomeCommand : Command
{
    public AwesomeCommand(IServiceProvider serviceProvider) : base(serviceProvider)
    {

    }

    public async Task OnExecuteAsync()
    {
        var amount = Context.Parameters.Get<int>(0);

        await PrintAsync($"{Actor.DisplayName} is {amount}x awesome!");
    }
}
```

First adjust the command to use the IStringLocalizer service:
```c#
[Command("awesome")]
public class AwesomeCommand : Command
{
    private readonly IStringLocalizer m_StringLocalizer;

    public AwesomeCommand(IStringLocalizer stringLocalizer, IServiceProvider serviceProvider) : base(serviceProvider)
    {
        m_StringLocalizer = stringLocalizer;
    }

    public async Task OnExecuteAsync()
    {
        var count = Context.Parameters.Get<int>(0);

        await PrintAsync(m_StringLocalizer["commands.awesome", new { Actor = Actor, Amount = amount }]);
    }
}
```

`commands.awesome` defines the key for the translation. By default, it equals to the path of the yaml file. You can use any path, it does not have to start with `commands`. See [configuration](../confiugration.md) for more about such paths.  

`new { Actor = Actor, Amount = amount }` passes the parameters for the translations.

We now need to add a new translation to the `translations.yaml`:
```yaml
commands: 
  awesome: "{Actor.DisplayName} is {Amount}x awesome!"
```

Notice how we can access properties of the Actor parameter by calling `Actor.DisplayName`. By default, OpenMod uses SmartFormat.NET for parsing parameters. See the [SmartFormat.NET wiki](https://github.com/axuno/SmartFormat/wiki) for more information. 