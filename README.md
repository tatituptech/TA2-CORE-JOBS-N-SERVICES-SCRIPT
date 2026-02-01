𐐘💥╾━╤デ╦︻ඞාTATITUPTECHDADEV𐐘💥╾━╤デ╦︻ඞා
𐐘💥╾━╤デ╦︻ඞාTATITUPTECHDADEV𐐘💥╾━╤デ╦︻ඞා
𐐘💥╾━╤デ╦︻ඞාTATITUPTECHDADEV𐐘💥╾━╤デ╦︻ඞා
# TA2-Core Framework

A complete FiveM server core framework similar to QBCore, providing essential functionality for roleplay servers.

## Features

- **Player Management System** - Complete player data handling with save/load functionality
- **Database Integration** - MySQL support via oxmysql with automatic table creation
- **Job System** - Configurable jobs with grades and payment structures
- **Money Management** - Cash and bank accounts with transaction tracking
- **Gang System** - Gang membership and hierarchy support
- **Item System** - Extensive item definitions with metadata
- **Vehicle System** - Vehicle spawning, properties, and management
- **Weapon System** - Weapon definitions with ammo types
- **Callback System** - Client-server synchronous communication
- **Command System** - Permission-based admin commands
- **Event-Driven Architecture** - Extensible event system for other resources

## Installation

### Prerequisites

- A FiveM server (Windows or Linux)
- MySQL database server
- [oxmysql](https://github.com/overextended/oxmysql) resource

### Setup Instructions

1. **Download and Extract**
   ```
   Download or clone this repository into your FiveM resources folder:
   /resources/[core]/ta2-core
   ```

2. **Database Configuration**
   - Create a MySQL database for your server
   - The framework will automatically create required tables on first run
   - Configure database connection in `server.cfg`:
   ```cfg
   set mysql_connection_string "mysql://username:password@localhost/database_name"
   ```

3. **Server Configuration**
   - Add to your `server.cfg`:
   ```cfg
   ensure oxmysql
   ensure ta2-core
   ```

4. **Framework Configuration**
   - Edit `config.lua` to customize:
     - Starting money amounts
     - Default spawn location
     - Player settings
     - Inventory settings
     - And more...

## Configuration

### Main Config (`config.lua`)

```lua
Config.StartingCash = 5000        -- Starting cash money
Config.StartingBank = 25000       -- Starting bank money
Config.DefaultJob = 'unemployed'  -- Default job for new players
Config.DefaultSpawn = vector4(-1035.71, -2731.87, 12.76, 0.0)
Config.MaxWeight = 120000         -- Max inventory weight (grams)
Config.MaxSlots = 41              -- Max inventory slots
```

### Adding Jobs

Edit `shared/jobs.lua` to add or modify jobs:

```lua
['yourjob'] = {
    label = 'Your Job Label',
    defaultDuty = true,
    offDutyPay = false,
    grades = {
        ['0'] = {
            name = 'Entry Level',
            payment = 50
        },
        ['1'] = {
            name = 'Manager',
            isboss = true,
            payment = 100
        }
    }
}
```

### Adding Items

Edit `shared/items.lua` to add new items:

```lua
['item_name'] = {
    name = 'item_name',
    label = 'Item Label',
    weight = 100,
    type = 'item',
    image = 'item_name.png',
    unique = false,
    useable = true,
    shouldClose = true,
    description = 'Item description'
}
```

## Usage

### For Server Admins

**Admin Commands:**
- `/givemoney [id] [cash/bank] [amount]` - Give money to a player
- `/setjob [id] [job] [grade]` - Set a player's job
- `/car [model]` - Spawn a vehicle

**Player Commands:**
- `/dv` - Delete nearby vehicle
- `/coords` - Show current coordinates

### For Developers

**Getting the Core Object:**

```lua
-- Server-side
local TA2Core = exports['ta2-core']:GetCoreObject()

-- Client-side
local TA2Core = exports['ta2-core']:GetCoreObject()
```

**Common Functions:**

```lua
-- Server-side
local Player = TA2Core.Functions.GetPlayer(source)
Player.Functions.AddMoney('cash', 100, 'paycheck')
Player.Functions.SetJob('police', 2)

-- Client-side
local playerData = TA2Core.Functions.GetPlayerData()
TA2Core.Functions.Notify('Hello World!', 'success')
TA2Core.Functions.SpawnVehicle('adder', function(vehicle)
    TaskWarpPedIntoVehicle(PlayerPedId(), vehicle, -1)
end)
```

**Callbacks:**

```lua
-- Server-side - Create callback
TA2Core.Functions.CreateCallback('myresource:getData', function(source, cb, data)
    -- Process data
    cb(result)
end)

-- Client-side - Trigger callback
TA2Core.Functions.TriggerCallback('myresource:getData', function(result)
    print(result)
end, data)
```

**Events:**

```lua
-- Server-side
RegisterNetEvent('TA2Core:Server:PlayerLoaded', function(PlayerData)
    print('Player loaded: ' .. PlayerData.citizenid)
end)

-- Client-side
RegisterNetEvent('TA2Core:Client:OnPlayerLoaded', function()
    print('Player loaded on client')
end)
```

## Exports

### Server Exports

```lua
exports['ta2-core']:GetCoreObject()
```

Gets the core object to access all TA2Core functions.

### Client Exports

```lua
exports['ta2-core']:GetCoreObject()
```

Gets the core object to access all TA2Core functions.

## Events Reference

### Server Events

- `TA2Core:Server:PlayerLoaded` - Fired when player data is loaded
- `TA2Core:Server:OnMoneyChange` - Fired when player money changes
- `TA2Core:Server:OnJobUpdate` - Fired when player job changes

### Client Events

- `TA2Core:Client:OnPlayerLoaded` - Fired when player is loaded
- `TA2Core:Client:OnMoneyChange` - Fired when player money changes
- `TA2Core:Client:OnJobUpdate` - Fired when player job changes
- `TA2Core:Client:OnGangUpdate` - Fired when player gang changes

## Database Structure

The framework automatically creates the following table:

**players table:**
- `id` - Auto-increment ID
- `citizenid` - Unique citizen identifier
- `license` - Rockstar license
- `name` - Player name
- `money` - JSON money data (cash, bank)
- `charinfo` - JSON character info (name, birthdate, etc.)
- `job` - JSON job data
- `gang` - JSON gang data
- `position` - JSON position data
- `metadata` - JSON metadata (hunger, thirst, licenses, etc.)
- `inventory` - JSON inventory data
- `last_updated` - Timestamp

## Support & Development

This is a base framework. Additional features can be added via separate resources that depend on TA2-Core.

**Recommended Additional Resources:**
- Inventory system (UI)
- Phone system
- Housing system
- Job-specific resources
- Vehicle shops
- And more...

## Credits

Created by TA2-Development  
Inspired by QBCore Framework

## License

Free to use and modify for your FiveM server.
