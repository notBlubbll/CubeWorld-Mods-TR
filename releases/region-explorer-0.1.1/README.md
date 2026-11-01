# Region Explorer 0.1.1 (first test version)

Lists the nearby towns, challenges, and landmarks the game has already generated and still holds in memory, on the left side of the vanilla M map, sorted by straight-line horizontal distance. Left-click a row to set it as the destination, close the map, and keep going.

0.1.1 fixed the invisible cursor over the explorer panel: the native cursor was drawn before the mod panel and got covered; now a white pixel hand is drawn last, only inside the explorer panel and with the game in the foreground. The fingertip and the list click test use the same coordinates; on closing the map or leaving the panel the original game shows the cursor again — no global cursor-state changes.

This version reads existing map data; it does not generate far-away maps from the seed and has no full region-boundary prediction. Not appearing in the list does not mean a place does not exist. Towns can be recognized earlier than shopkeeper entities; challenges are listed only when the original record actually has the quest field — not every landmark can be called a boss. The original map save holds no live quest state, and the boss state still needs in-game verification.

## Usage

1. Installs to the `playtest` copy by default; the formal copy is never auto-installed. After the game and launcher fully exit, run `Install-RegionExplorer.ps1` from the root.
2. Turn on "区域探索" (Region Explorer) in the existing mod manager and start via the original playtest entry yourself. Do not install or hot-plug into a running game.
3. After entering a character and world press M and pick "Towns", "Challenges", or "Landmarks"; left-click a place in the list to set the target. It reads once every two seconds, and paging shows more places.
4. With Navigation 0.3.6 also enabled, they share the original top direction bar and one down arrow; no extra map nameplates. Map compatibility updates go through `Install-TownLabels.ps1` independently and are not bundled by this installer.
5. With Navigation disabled or absent, this module shows its own bottom-right direction/distance hint and single map arrow. With the old Navigation installed, first update the map to 0.3.6 alone, then enable both, to avoid the old navigation leaving a manual target behind too.
6. "Cancel explorer target" at the list bottom clears only this feature's target. When running together with the new map, a later manual marker wins; the explorer target is not re-seized every frame.

Place names are read from the original program's runtime generation word lists; local Chinese-game screenshots already show Chinese place names, and other localization combos still need confirmation. No translation was invented. Numbers use Navigation's game coordinate unit `m`, not yet calibrated to real meters. Targets are the place center's XY — not guaranteed to be a building entrance or the boss entity's exact spot, and no obstacle pathing is provided.

## Independent boundaries

| Item | Value |
| --- | --- |
| DLL | `mods/region_explorer.dll` |
| manifest ID | `local.cubeworld.region_explorer` |
| Manager ID | `RegionExplorer`, Chinese name "区域探索" |
| Switch | `region_explorer.enabled`, in the shared `mods.ini`; the other keys are untouched |
| Data | `data/region_explorer/world_<seed>_<namehash>/target.bin` |
| Capabilities | Overlay, RawMem only; no Writes or RawHooks declared |
| Read limits | ±3 map storage blocks around the player, at most 49 blocks of 64 records each |

Use the existing `Manage-Mod.ps1 -Mod RegionExplorer -Action Disable/Enable/Uninstall`. Disabling keeps the DLL; removal moves this DLL into the project backup, keeping the data and all saves. Map, potion, and explorer each toggle independently. The existing old shared launcher still requires the map DLL file; the map can be disabled on its own, and removing the map directly stays locked by the original management flow — this module cannot bypass it.

## Build and checks

Run `Build-RegionExplorer.ps1` from the root — build and checks only, no install, no game start. `research/inspect_explorer.py --saved` checks the playtest map save read-only in SQLite mode; `--live` reads only the verified game-process path. Sources and offset basis in [SOURCES.md](SOURCES.md). Current evidence and the remaining in-game acceptance are in `research/evidence/explorer-0.1.0-validation.md`.
