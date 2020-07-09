# Localization
Localization allows users to customize and translate your plugin's messages.  
This is achieved through the translations.yaml file and the IStringLocalizer service.

!!! Note
    The **RootNamespace** and **AssemblyName** need to be the same, if **RootNamespace** is not the same that **AssemblyName**, the `IStringLocalizer` will not work.
    
## Adding the translations.yaml
Create a new file called "translations.yaml" inside your project's root directory.  
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

First, adjust the command to use the IStringLocalizer service:
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

        await PrintAsync(m_StringLocalizer["commands:awesome", new { Actor = Actor, Amount = amount }]);
    }
}
```

`commands:awesome` defines the key for the translation. The default IStringLocalizer uses the key for the path inside the yaml file. You can use any valid path, such as `messages:awesome`, just `awesome`, etc.

`new { Actor = Actor, Amount = amount }` sets the arguments for the translations.

Finally add the translation to the `translations.yaml`:
```yaml
commands: 
  awesome: "{Actor.DisplayName} is {Amount}x awesome!"
```

Notice how we can access the properties of the Actor parameter by calling `Actor.DisplayName`. The default IStringLocalizer uses SmartFormat.NET for parsing arguments. See the [SmartFormat.NET wiki](https://github.com/axuno/SmartFormat/wiki) for more information. 
