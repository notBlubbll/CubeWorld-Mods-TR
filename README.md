# CubeWorld-Mods

Source code for the standalone Cube World 2013 Alpha mods and the Windows mod manager, including the shared launcher.

**This is a development source snapshot.** Each feature can be built and managed separately, and the manager gives every mod its own on/off switch. Install paths in the public copy have been unified to example directories; strict file hash verification is still in place, and this has not yet become a general-purpose installer for all players. No universal installer package is currently provided.

## Features

| Mod | Current source version | Features | DLL |
| --- | --- | --- | --- |
| [Navigation](navigation/README.md) | 0.3.8 | Map waypoints, direction and distance, town labels and the shop guide | `waypoint.dll` |
| [Potion Shop](potion_shop/README.md) | 0.2.0 | Sells health potion tiers that follow the recipes the character has unlocked | `potion_shop.dll` |
| [Region Explorer](region_explorer/README.md) | 0.1.6 | Nearby loaded towns, landmarks, challenges, and limited boss class info | `region_explorer.dll` |
| [Map Light](map_light/README.md) | 0.1.1 | Lights up the full world map when opened at night | `map_light.dll` |
| [Inventory Sort](inventory_sort/README.md) | 0.1.7 | Grouped equipment display, Chinese section labels, and preserved empty slots | `inventory_sort.dll` |
| [Fast Travel](fast_travel/README.md) | 0.1.4 | Visit history, place names and notes; F8 to pick a destination | `fast_travel.dll` |
| [Batch Actions](batch_actions/README.md) | 0.1.2 | Quantity selection when buying empty bottles or crafting water/health potions | `batch_actions.dll` |
| [Boss Draw](boss_draw/README.md) | 0.1.7 | Keeps original loot, adds an extra draw after boss kills; F9 manual draw | `boss_draw.dll` |

The [Mod Manager](mod_manager/README.md) is a C# Windows Forms application that lists registered mods, toggles each one independently, provides backup entries, and invokes the launcher. It does not inject DLLs itself, and it does not auto-detect arbitrary community mods.

## Architecture and compatibility scope

The shared parts use [Qube-Loader](https://github.com/qad3n/Qube-Loader) and this project's `NavigationLauncher.exe`. Each feature is a standalone DLL; turning one of them off must not stop the others from working. Pinned source versions of Qube-Loader, Dear ImGui, and MinHook are kept in the repository, together with their original licenses and a record of local changes.

This targets only the verified Windows 32-bit **2013 Alpha** client. Actual client SHA-256:

```text
84A7A132A84D4282338E7EA45A1940D32066D64E39CBAFED8F2418D5A6DC30BF
```

Not compatible with the 2019 Steam version or Omega. Unverified clients, loaders, and function conflicts are rejected. There is no promise that the classic Mod Launcher's DLLs or other community mods can be dropped into this framework as-is.

## Building and usage

See [Build notes and current limitations](BUILDING.md). You need Windows, the Visual Studio 2022 C++ desktop development tools, and CMake; the manager uses the .NET Framework C# compiler that ships with Windows.

The source repository does not include the game itself, game assets, fonts, saves, player locations, live memory data, private logs, or prebuilt DLLs/EXEs. Modules read the player's installed game assets or system fonts at runtime. Build artifacts belong in GitHub Releases and must match the corresponding source version.

For Boss Draw 0.1.7, real new-kill registration, prize claiming, and the save/restart round trip — as well as real combined runs of all mods — are still pending acceptance; passing offline checks does not mean those are done. Each module's docs keep the necessary technical history; personal characters, worlds, compensation records, and install logs are not public; directories are examples only.

## Directory layout

```text
navigation/ potion_shop/ region_explorer/ map_light/
inventory_sort/ fast_travel/ batch_actions/ boss_draw/
mod_manager/             Windows mod manager
external/Qube-Loader/    pinned dependency sources and licenses required for the build
research/               previews, check tools, and necessary research notes
releases/               historical version notes, install verification metadata, and licenses
Build-*.ps1             the maintainer's existing build entry points
Install-*.ps1           the maintainer's existing install entry points; depend on local directories and release packages
Manage-Mod.ps1          per-mod enable/disable, backup, and removal entry point
SOURCE_SNAPSHOT.json    snapshot of this workspace and its dependency versions
```

## License and sources

This repository's own program source code is released under [GNU GPLv3](LICENSE); third-party components keep their respective licenses and attribution, see [Third-party sources](THIRD_PARTY.md). The game itself, its assets, and its trademarks belong to their respective rights holders and are not covered by this repository's license. This is an unofficial community project.
