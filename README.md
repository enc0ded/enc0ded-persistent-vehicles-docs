To buy a copy of this mod please visit: https://fivem-mods.tebex.io/package/4499436

# enc0ded-persistent-vehicles - version 3

This mod prevents vehicles from disappearing in OneSync multiplayer servers. It can also respawn vehicles in their previous location after a server restart.

## Requirements
> FiveM Version >=5181 OneSync - This mod will NOT work without OneSync.

## Installation
Place the enc0ded-persistent-vehicles folder in your resources folder. Open config.lua and configure to your needs. Start the resource. See Usage below on how to make a vehicle persistent.

```bash
ensure enc0ded-persistent-vehicles
```

## Usage
IMPORTANT: You must only tigger 'persistent-vehicles/register-vehicle' after you set a lisense plate to a vehicle, or the vehicle will be registered with the wrong plate and WILL duplicate as mod uses plates to know if a vehicle exists or not.

All vehicles must have a unqiue lisense plate.
To make a vehicle persistent, pass its entity to the event below in a client script.

Set light to true to only remember important props, good for work vehicles. Set forgetAfter (in seconds) to forget vehicle after specified time. 
Set statebag key value pair to register statebag server side. See "State Bag Support" below for more information.
light, forgetAfter and state are optional params and can be omitted.
```lua
# client event
TriggerEvent('persistent-vehicles/register-vehicle', entity, light, forgetAfter, state)
```

Stop a vehicle from being persistent and allow it to be removed as normal. Does not delete the vehicle.
```lua
# client  event
TriggerEvent('persistent-vehicles/forget-vehicle', entity)
```

Stop vehicle from being persistent and delete the vehicle. Call this when a player puts away a vehicle and remove client side DeleteEntity/DeleteVehicle function. Also remember to call this on your admin delete vehicle commands.
```lua
# client  event
TriggerEvent('persistent-vehicles/delete-vehicle', entity)
```

If you are using TXAdmin you do not need to run this event, just ensure txadin = true in config.lua!
Otherwise, if you enable repopulate on reboot then you need to call the server event below before your server shuts down.
```lua
# server  event
TriggerEvent('persistent-vehicles/save-vehicles-to-file')
```

## Server Export Functions
Return vehicle data known by the server. If vehicle is not persistent will return false.
First arguement is the vehicle plate, second is a table of data to include: pos, props, entity, netId, trailer and forgetOn
```lua
exports['enc0ded-persistent-vehicles']:GetVehicleData('PLATE', {'pos', 'props'})
```

## Game Admin Commands
Register vehicle player is in
```bash
pv-register
```
Unregister and delete vehicle player is in
```bash
pv-delete
```

## Server Console Commands
Unpersist all vehicles
```bash
pv-forget-all
```
 Unpersist particular vehicle - example: pv-forget-veh "ABC 123"
```bash
pv-forget-veh "plate"
```
Cull persistent vehicles
```bash
pv-cull <number of vehicles>
```
Get number of vehicles spawned on the map
```bash
pv-num-spawned
```
Get number of vehicles registered as persistent
```bash
pv-num-registered
```
Toggle console debugging messages
```bash
pv-toggle-debugging
```
Save all persistent vehicles to file. Must be called before manually restarting server outside server restart times.
```bash
pv-save-to-file
```

## State Bag Support

Pass a table with key/value pairs as the fourth paramater to register state bags on vehicle entities. The state will be registered server side to ensure it is propagated to all clients.
```lua
TriggerEvent('persistent-vehicles/register-vehicle', entity, false, false, {key1 = 'value',  key2 = 'value'})
```

You can then update a vehicle's statebags after registration, set the state client side and the new state will be propagrated to all clients and the server automatically. 
```lua
Entity(entity).state:set('key1', 'new value', true)
```
IMPORTANT: For a statebag to be propagated correctly to the server and all clients, you must pass its key/value pair to the register-vehicle function 
or set the state server side before attempting to change the state client-side. 

## License
All rights reserved. You must purchase a license from https://fivem-mods.tebex.io
