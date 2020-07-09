# Data Store
The IDataStore service provides a way of saving and loading persistent data.  
The default data store uses yaml files for serialization.

## Example
Assume you want to save and load the following class:
```c#
[Serializable]
public class PlayersData
{
   public List<string> OwnerNames { get; set; }
}
```

```c#
public class MyPlugin : OpenModUniversalPlugin
{
    // Defines the key for the data. The default data store uses the key as the file name for the yaml file.
    // In this case, the file will be named owners.data.yaml
    public const string OwnersKey = "owners";
    private readonly IDataStore m_DataStore;
    
    public MyPlugin(IDataStore dataStore, IServiceProvider serviceProvider) : base(serviceProvider)
    {
        m_DataStore = dataStore;
    }

    public async Task OnLoadAsync()
    {
        // first check if the data exists and create it if it does not exist
        if(!await m_DataStore.ExistsAsync(OwnersKey))
        {
            await SeedData();
        }

        var data = await m_DataStore.LoadAsync<PlayersData>(OwnersKey);        
        // do something with data
        await m_DataStore.SaveAsync<PlayersData>(OwnersKey, data);
    }

    private async Task SeedData()
    {
        // create default data
        await m_DataStore.SaveAsync(OwnersKey, new PlayersData
        {
            OwnerNames = new List<string> { "Trojaner" }
        });
    } 
}
```

## Implementing your own data store
You can replace OpenMods default yaml data store by implementing the `IDataStoreFactory` interface and adding the `[ServiceImplementation]` attribute to the concrete class.
