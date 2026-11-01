# Navigation 0.3.6: Region Explorer compatibility

Adds the optional Region Explorer target handoff and left-panel yielding. When Region Explorer picks a spot, they share one navigation target and one down pixel arrow; with Region Explorer absent or disabled, the original map waypoints, shop guide, and the 0.3.5 duplicate-inn fix are kept.

This is the map module's own compatibility update and does not make Region Explorer a required dependency. Region Explorer's DLL, switch, and data are managed by the separate `Install-RegionExplorer.ps1` / `Manage-Mod.ps1 -Mod RegionExplorer`.

The root `Install-TownLabels.ps1` backs up and updates the playtest copy's `mods/waypoint.dll` by default; the game and launcher must be fully exited. The original game, saves, config, potion, and the other mods are kept. The formal copy is updated only when the user explicitly says so. Installing never starts the game.

The 51 navigation checks, 44 town checks, and the real DLL's messaging/storage failure checks in the synthetic SDK served as offline validation; the full record is in `research/evidence/explorer-0.1.0-validation.md`. In-game place selection, map toggling, and the mod combinations still need live testing.
