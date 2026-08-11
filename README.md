![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/head.png)

# DCS Air Drops for the C130j v0.6
This is a DCS mission .lua script that allows c130 Air Drops to be called in from the radio menu. It also allows the player to drop CDS crates and use map markers to "manufacture" ground vehicles from dropped CDS supply crates.

This script includes the spawning of c130's, which will carry 2 weapons platforms for deployment. It has multiple vehicle types including Tanks, APCs, Humvees, SAM systems, and FARP equipment. Once the radio command is issued it will give you a map marker code so you can create a mission map marker in the f10 menu with that code. Manufactured FARPs are fully functional Heliports with ATC radio comms, refuel/rearm, and dynamic aircraft spawn (with hot starts).

If you would like to troubleshoot in the lua script you can turn debugging on which will output messages.

# Instructions

## Configuration Options
The script includes several configuration options at the top of the file:

```lua
local CONFIG = {
    debug = false,                    -- Enable debug messages
    use_test_crates = true,          -- Enable test crates for easier testing
    production_mode = false,         -- Reduce overhead and debug output
    enable_air_drops = true,         -- Enable/disable air drop functionality
    enable_make_command = true,      -- Enable/disable "make" commands
    enable_npc_drops = true,         -- Enable/disable NPC air drops
}
```

### FARP Configuration
FARPs are manufactured as fully functional Heliports. Configure them in `CONFIG.FARP_SPAWN`:

```lua
FARP_SPAWN = {
    name_prefix = "FARP",           -- FARP naming: e.g. "FARP ALPHA", "FARP BRAVO" ...
    shape_name = "FARP",            -- Visual: "FARP" (1 pad), "FARPs" (4 pads) or "invisiblefarp"
    callsign_id = 1,                -- Heliport callsign index (1-10)
    frequency = 127.5,              -- Heliport radio frequency in MHz (VHF AM/FM band)
    modulation = 0,                 -- 0 = AM, 1 = FM
    dynamic_spawn = true,           -- Allow dynamic aircraft spawn from this FARP
    allow_hot_start = true,         -- Allow hot starts for dynamically spawned aircraft
    airframes = {                   -- Aircraft available in the FARP warehouse: DCS unit typeName = quantity
        ["AH-64D_BLK_II"] = 5,     -- Apache AH-64D
        ["Ka-50_3"] = 5,           -- Ka-50 Black Shark 3
        ["UH-60L"] = 5,            -- UH-60L Black Hawk
    },
    fuel_jetfuel = 50000,           -- Jet fuel (liters) in warehouse
    fuel_avgas = 20000,             -- Aviation gasoline (liters) in warehouse
}
```

Airframe keys must be the exact DCS unit type names (not display names) for them to appear in the FARP's dynamic spawn inventory.

Spawned FARPs are named sequentially using the `name_prefix` and NATO phonetic words (`FARP ALPHA`, `FARP BRAVO`, `FARP CHARLIE`, ...), falling back to numbers beyond ZULU.

## Setup
1. Place the `Air Drop.lua` script in your mission's MISSION SCRIPTS folder
2. Load the script in the mission editor using the "Do Script" action

Alternatively:
1. Open the example mission file `Air Drop.miz` in the mission editor
2. Click the Set Rules for trigger icon on the left hand nav menu (3 Down from the text "MIS")
3. Note the ONCE trigger is set. Click on it
4. Note the Time More is set to trigger on load
5. Click the DO SCRIPT FILE and make sure it is linked to the `Air Drop.lua` script

## Usage

### Radio Menu Air Drops
- Use the radio menu to call in drops (costs CMD points if Player Tracker system is loaded)
- Place map markers named `dp-alpha`, `dp-bravo`, etc. for drop zones (these are generated dynamically)

### Manual C-130J Drops
- Spawn cargo containers using the C-130J mod (or any static object with the correct name pattern) 
- Fly and drop them from the C-130J
- Use map markers to spawn vehicles from nearby landed crates:
  - `make supplytruck` - Truck M818 (1 material required)
  - `make tank` - M1 Abrams Tank (4 materials required)
  - `make apc` - M1126 Stryker ICV (2 materials required)  
  - `make humvee` - M1025 HMMWV (2 materials required)
  - `make mbt` - M1A2C SEP V3 (5 materials required)
  - `make mlrs` - M270 MLRS (3 materials required)
  - `make srsam` - Short Range SAM (5 materials required)
  - `make lrsam` - Long Range SAM (5 materials required)
  - `make farp` - Forward Arming and Refueling Point (4 container_cargo OR 7 uh1h_cargo) - spawns a fully functional FARP

### Test Crates
- Enable `use_test_crates = true` in configuration to use pre-placed test crates
- Test crates with names matching `cds_crate-test-*` are automatically configured as landed
- Useful for mission testing without requiring actual air drops

### CMD Point Costs (if Player Tracker available)
- 2 units (1 C-130): 20 CMD points
- 4 units (2 C-130s): 40 CMD points  
- 6 units (3 C-130s): 60 CMD points
- 8 units (4 C-130s): 80 CMD points

### Manufacturing Requirements
- **Supply Truck (M818)**: 1 material
- **Tank (M1 Abrams)**: 4 materials
- **APC (M1126 Stryker ICV)**: 2 materials
- **Humvee (M1025 HMMWV)**: 2 materials
- **MBT (M1A2C SEP V3)**: 5 materials
- **MLRS (M270)**: 3 materials
- **Short Range SAM**: 5 materials
- **Long Range SAM**: 5 materials
- **FARP**: 4 container_cargo OR 7 uh1h_cargo materials

Container detection includes both dynamic containers (dropped from aircraft) and static containers (pre-placed in mission).

## Version 0.6 Changes

### Functional FARP Spawning
- **Real Heliports**: Manufactured FARPs now spawn as functional Heliports (ATC radio comms, refuel/rearm, dynamic aircraft spawn) instead of inert statics
- **Dynamic Spawn**: FARPs appear in the F10 dynamic spawn menu with hot start support
- **Warehouse Inventory**: Configured airframes and fuel are automatically added to each spawned FARP's warehouse via the new `CONFIG.FARP_SPAWN` block
- **Reliable Provisioning**: Warehouse setup retries until the FARP is ready, with per-airframe inventory verification logged

## Version 0.5 Changes

### Performance Improvements
- **Event-driven container detection**: Containers are now detected immediately when spawned instead of periodic scanning
- **Real-time marker processing**: Map marker commands are processed instantly using S_EVENT_MARK_CHANGE
- **Optimized monitoring**: Reduced CPU overhead by eliminating redundant scans

### Enhanced Features  
- **Test crate support**: Configurable test crates for easier mission development
- **Static container scanning**: Detects pre-placed containers during initialization
- **Improved vehicle configurations**: Updated to M1126 Stryker ICV, enhanced skill levels
- **Better error handling**: More informative debug messages and error reporting
- **Hybrid detection system**: Combines event-driven and static scanning for comprehensive coverage

### Bug Fixes
- Fixed container detection for static objects that don't trigger birth events  
- Improved container state tracking and position updates
- Enhanced FARP material validation using container type names
- Better coalition detection for spawned containers

![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/cds.png)
![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/cds2.png)
![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/farp.png)
![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/farp1.png)