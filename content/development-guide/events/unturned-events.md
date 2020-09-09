# Unturned Events

## Animal Events
These events are stored within the `OpenMod.Unturned.Animals.Events` namespace.

| **Event**                          | **Fired when**                                                               | **Cancellable** |
|------------------------------------|------------------------------------------------------------------------------|-----------------|
| UnturnedAnimalSpawnedEvent         | After an animal spawns (is revived or new instance added)                    | No              |
| UnturnedAnimalAddedEvent           | After a new animal instance is added (triggers spawned event)                | No              |
| UnturnedAnimalRevivedEvent         | After an animal is revived (triggers spawned event)                          | No              |
| UnturnedAnimalAlertingEvent        | Before an animal is alerted                                                  | Yes             |
| UnturnedAnimalAttackingEvent       | Before an animal targets a player/point for attack (triggers alerting event) | Yes             |
| UnturnedAnimalAttackingPlayerEvent | Before an animal targets a player for attack (triggers attacking event)      | Yes             |
| UnturnedAnimalAttackingPointEvent  | Before an animal targets a point for attack (triggers attacking event)       | Yes             |
| UnturnedAnimalFleeingEvent         | Before an animal flees                                                       | Yes             |
| UnturnedAnimalDamagingEvent        | Before an animal is damaged                                                  | Yes             |
| UnturnedAnimalDyingEvent           | Before an animal takes a fatal amount of damage                              | Yes             |
| UnturnedAnimalDeadEvent            | After an animal dies                                                         | No              |

## Building Events
These events are stored within the `OpenMod.Unturned.Building.Events` namespace.

Barricade and structure events derive from their buildable counterparts. Whenever a barricade or structure event is emitted, the buildable version will be emitted as well.

| **Event**                        | **Fired when**                                         | **Cancellable** |
|----------------------------------|--------------------------------------------------------|-----------------|
| UnturnedBuildableDeployedEvent   | After a buildable is deployed                          | No              |
| UnturnedBuildableSalvagingEvent  | Before a buildable is salvaged                         | Yes             |
| UnturnedBuildableDamagingEvent   | Before a buildable is damaged                          | Yes             |
| UnturnedBuildableDestroyingEvent | Before a buildable takes enough damage to be destroyed | Yes             |
| UnturnedBuildableDestroyedEvent  | After a buildable is destroyed                         | No              |
| IUnturnedBarricadeEvent          | Any barricade event is fired                           |                 |
| UnturnedBarricadeDeployedEvent   | After a barricade is deployed                          | No              |
| UnturnedBarricadeSalvagingEvent  | Before a barricade is salvaged                         | Yes             |
| UnturnedBarricadeDamagingEvent   | Before a barricade is damaged                          | Yes             |
| UnturnedBarricadeDestroyingEvent | Before a barricade takes enough damage to be destroyed | Yes             |
| UnturnedBarricadeDestroyedEvent  | After a barricade is destroyed                         | No              |
| IUnturnedStructureEvent          | Any structure event is fired                           |                 |
| UnturnedStructureDeployedEvent   | After a structure is deployed                          | No              |
| UnturnedStructureSalvagingEvent  | Before a structure is salvaged                         | Yes             |
| UnturnedStructureDamagingEvent   | Before a structure is damaged                          | Yes             |
| UnturnedStructureDestroyingEvent | Before a structure takes enough damage to be destroyed | Yes             |

## Environment Events
These events are stored within the `OpenMod.Unturned.Environment.Events` namespace.

| **Event**                    | **Fired when**                    | **Cancellable** |
|------------------------------|-----------------------------------|-----------------|
| UnturnedDayNightUpdatedEvent | After the day/night cycle updates | No              |
| UnturnedWeatherUpdatedEvent  | After the weather updates         | No              |

## Player Events

### Bans
These events are stored within the `OpenMod.Unturned.Players.Events.Bans` namespace.

| **Event**                      | **Fired when**                          | **Cancellable** |
|--------------------------------|-----------------------------------------|-----------------|
| UnturnedPlayerBanningEvent     | Before a player is banned               | Yes             |
| UnturnedPlayerBannedEvent      | After a player is banned                | No              |
| UnturnedPlayerUnbanningEvent   | Before a player is unbanned             | Yes             |
| UnturnedPlayerUnbannedEvent    | After a player is unbanned              | No              |
| UnturnedPlayerCheckingBanEvent | Before a player's ban status is checked | Yes             |

### Chat
These events are stored within the `OpenMod.Unturned.Players.Events.Chat` namespace.

| **Event**                         | **Fired when**                                        | **Cancellable**                 |
|-----------------------------------|-------------------------------------------------------|---------------------------------|
| UnturnedPlayerChattingEvent       | Before a player chats                                 | Yes                             |
| UnturnedServerSendingMessageEvent | Before the server displays a chat message to a player | No, but there modifiable fields |
