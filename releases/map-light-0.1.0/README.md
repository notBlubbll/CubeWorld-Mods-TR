# Map Light 0.1.0

Makes the world map opened with **M** use fixed lighting, making terrain, towns, and challenge spots easier to read at night. After closing the map, the next frame goes back to the vanilla night scene. The first version's brightness is still pending in-game acceptance.

This is the standalone mod `mods/map_light.dll`, manager ID `MapLight`, module ID `local.cubeworld.map_light`. It does not depend on Navigation, Region Explorer, or the Potion Shop, and each can be toggled separately. It shares the existing Qube Loader and launcher; installing it does not replace them.

## Usage

After fully exiting the playtest game and launcher, pick the "playtest copy" in the existing "魔方世界模组管理器" (Cube World mod manager) and turn on "Map Light". The player clicks "启动游戏" (start game), enters a character and world, and presses M. Turning the switch off restores the vanilla map lighting on the next launch.

The mod has no in-game panel or new hotkeys; it does not change map interaction, time, lanterns, items, monsters, saves, or the other mods' data, and it makes no permanent changes to the game program. It only makes the original render function read this mod's fixed color parameters while the world map is open. The minimap and the normal walking view keep reading the original parameters.

## Build, install, and removal

```powershell
.\Build-MapLight.ps1
.\Install-MapLight.ps1 -ValidateOnly
.\Install-MapLight.ps1
.\Manage-Mod.ps1 -Mod MapLight -Action Disable
.\Manage-Mod.ps1 -Mod MapLight -Action Enable
.\Manage-Mod.ps1 -Mod MapLight -Action Uninstall
```

By default it only touches the project's `playtest` copy; before installing it verifies the game and the loader, confirms they exited, backs up the full Save, and checks the other files. Installing only adds this mod's DLL and an install record, preserving `mods.ini` byte-for-byte; an existing `map_light.enabled=0` is kept too. The independent toggle key is `map_light.enabled`; when missing it defaults to enabled per the loader rules; error counters are not reset. No new runtime data files. Any future or existing `config/map_light.ini` and `data/map_light/` are reserved for this mod.

Disabling keeps the DLL; removal moves this DLL into the project backup and disables only this switch, keeping the other mods, configs, and saves. The formal directory additionally needs the user's explicit authorization. No in-run replacement, disabling, or removal, and the game is never started automatically.

## Implementation and validation boundaries

Only the verified 2013 Alpha client is supported. At load it checks the PE identity, the native lighting function entry, the stack cleanup, and the actual call sites; an unknown version or an entry already changed by other hooks is refused. The SDK needs the RawMem, RawHooks, and Writes permissions; Writes only temporarily changes the passed-in pointer, with no direct writes to game memory. The fixed lighting comes from the vanilla world-map landmark draw parameters; evidence in [SOURCES.md](SOURCES.md).

The offline checks run the real DLL with a synthetic SDK injected, verifying map open/close, the independent identity, parameter permissions, bad pointers, unknown versions, disable cleanup, and no direct memory writes; the native game and GPU drawing were never run. The manager, install, and removal checks run in a separate isolated directory. The real night brightness, the night scene after closing the map, and running alongside the other mods still need player acceptance — see validation record (local validation records are not published with the source).

This project uses the Qube Loader SDK; the license is kept with the release package as GPL-3.0; no original game assets or programs are distributed.
