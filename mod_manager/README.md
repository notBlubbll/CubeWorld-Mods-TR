# Cube World Mod Manager

A local Windows desktop app that lets the user independently enable or disable the Navigation, Potion Shop, Region Explorer, Map Light, Inventory Sort, Fast Travel, Batch Actions, and Boss Draw mods. The interface keeps the deep-blue palette, the blond icon, and a few blocky elements, with a compact list and thin-line controls.

## Usage

Double-click the project root's `魔方世界模组管理器.lnk`, or `启动模组管理器.cmd`. It manages the playtest copy by default; the formal play copy can be selected explicitly.

Since 2026-10-29 it supports an ordinary desktop window: drag the edges to resize; the title bar minimizes, maximizes, or restores. Click "Fullscreen F11" or press F11 for fullscreen; Esc or F11 returns to the window position, size, and maximized state from before entering.

Text, icons, and switches keep their size with the Windows display scale and do not shrink with the mod count or window size. The mod list scrolls independently and auto-flows into two columns in wide windows; the copy selector, status hints, and the bottom "start game" bar stay fixed. Adding mods only grows the list — the fixed UI no longer stretches.

The same day it was tightened per UI feedback to a default 920 × 700, minimum 680 × 480 logical window: mod rows 88 high, mod titles 17, body 13, status 12 logical pixels. Descriptions appear once in the list, removing duplicate hover tooltips; the switch directly shows the normal on/off state, with missing-file, unknown, and lock reasons still shown in place. A running old window must be closed and reopened from the original entry to show the new UI. Compact-version validation is in `../research/evidence/mod-manager-compact-20261029.md`.

The switches show the saved config in the target directory. After toggling, the actual state is re-read; when it cannot be confirmed it shows unknown and locks — success is not shown early. Changes take effect on the next game start.

After adjusting the switches, click the gold "启动游戏" (start game) at the bottom-right to start the game through the selected copy's `NavigationLauncher.exe` and load the enabled mods. Opening the manager or switching copies never starts the game. When the game is running, the process state is unclear, the launcher is missing, or checks fail, the button locks with a reason. After starting, the manager can be closed — the game keeps running; the manager never terminates the game.

The start check recognizes the playtest copy's old launcher, the formal copy's recorded 740-fix build, and the new standalone-mod-capable launcher — all verified by fixed SHA256. The two old builds still require the map DLL file; recognizing a compatible version never upgrades the launcher along the way.

A feature whose DLL is missing cannot be enabled. While running, unclear, or failing checks, the switch locks. Disabling keeps the mod files, config, data, and saves; the existing management script backs up independently before changes.

Potion Shop 0.2.0 keeps the original "药水商店" entry and the `potion_shop.enabled` switch. Each shop opening sells the health potion one tier below the character's best craftable, auto-updating with progression; the tiers and prices are in `../potion_shop/README.md`. The manager stores no character level or potion tier.

Region Explorer 0.1.5 keeps the original `RegionExplorer` entry, `mods/region_explorer.dll`, and `region_explorer.enabled`. The Challenges page adds the boss class's confirmed, limited prediction, and unknown; this pass updated only the explorer DLL, adding no manager entry and changing no manager code. Before and after, the five mods' original switches were verified unchanged, Map Light stays off, and the disable/remove flow is unchanged.

Region Explorer 0.1.2 kept the "区域探索" entry with manager ID `RegionExplorer`, mapping to `mods/region_explorer.dll` and `region_explorer.enabled`. The vanilla region-level white/cyan/red difficulty colors were added; the original paths and toggling still match, with no new switches and no manager-program changes; the map and potion keep their own original switches. The feature's scope and limits are in `../region_explorer/README.md`.

Map Light uses the fourth entry, "地图照明", manager ID `MapLight`, mapping to `mods/map_light.dll` and `map_light.enabled`. 0.1.0 was wired in and the original entry rebuilt. 0.1.1 keeps the same entry, switching to automatic night-time lighting for the world map with daytime vanilla; closing the map restores the vanilla night. The 0.1.1 candidate manager is kept at `../releases/map-light-0.1.1/ModManager.exe`. The Inventory Sort integration rebuilt the original manager into a five-row switch panel; the in-game light DLL is still 0.1.0 and was not replaced this time. Details in `../map_light/README.md`.

## Implementation and build

Inventory Sort 0.1.6 keeps the fifth entry, "背包分类", manager ID `InventorySort`, mapping to `mods/inventory_sort.dll` and `inventory_sort.enabled`. Chestplates, gloves, and so on show independent Chinese titles; pickups can be placed back into visible slots, and sales leave the empty slot without fill-in; positions persist for the current game session, and the vanilla cell size, per-row capacity, and wrapping are kept. The original double-click entry now contains eight features with their own switches, without duplicating the inventory entry. The current window is resizable with an independently scrolling list; the checks for the earlier six-row fixed UI are kept in the Fast Travel historical validation record. Source and notes in `../inventory_sort/README.md`.

- `ModManager.cs`: the Windows Forms UI, using the Windows-bundled .NET Framework — no browser or local network service needed.
- `Get-ModManagerState.ps1`: the read-only JSON status interface; interprets the config per the current loader's defaults and boolean rules.
- `Start-ModGame.ps1`: re-checks the state on each click and calls only the verified shared launcher in the target directory; it changes no config and updates no launcher. It returns the launch-request result — a successful request is not the same as the game reaching its main menu.
- `../Manage-Mod.ps1`: the single write entry for mod configs. The UI implements no parallel config-changing flow and loads no game or mod DLLs itself.
- `../Build-ModManager.ps1`: builds the windowless program with the system C# compiler and creates the local shortcut.

The app dynamically finds the installed PowerShell 7, including the machine's existing Codex runtime. When not found it falls back to Windows PowerShell 5.1 read-only queries and locks writes; it downloads nothing and changes no system security settings.

The main title font may read the local `playtest/resource1.dat` — used only as this process's private font; the resource is never copied or distributed, and no system font is installed. The list titles, descriptions, and buttons use system Chinese fonts for compact layout and readability.

The launcher uses a voxel-adventurer icon made referencing Picroma's official site characters and logo, applied to the program, shortcut, taskbar, and window header. The transparent original, multi-size ICO, reference sources, and generation prompt are kept in `assets/`; the build embeds the resource and does not depend on the local image-generation directory.

## Integration duties for new mods

When adding a standalone mod, the developer responsible for that feature must also complete the existing manager integration. That is the project's default delivery scope, not waiting for the user to remind. The delivery standard is in the root `AGENTS.md` and in `MODS.md`'s "Default delivery requirements for new mods".

The integration points in the current implementation:

| Location | What must be kept in sync |
| --- | --- |
| `Get-ModManagerState.ps1` | Allowed modules, DLL path checks, Chinese names/descriptions, install and on/off states, safe-lock reasons. |
| `../Manage-Mod.ps1` | Allowed modules and the fixed file mapping; change only the selected module's toggle key, keeping the backup, game-exit, and data-preservation checks. |
| `ModManager.cs` | New entry, independent switch events, and status updates; add the new row to `modRows` and let the scrolling list lay it out. Do not lower `Skin.Scale` by entry count or grow the fixed window height. |
| The matching build/install scripts and `../Prepare-Playtest.ps1` | Build the standalone DLL, provide the selected components' preparation and install flow, keep the other components. |
| `../Build-ModManager.ps1` | Rebuild the manager and verify the original double-click entry still works. |

After integration, verify the new feature's independent toggling, the original features staying unchanged, the missing-file and running locks, and the backup and config/data preservation. Source or a DLL without a manager switch is still an unfinished delivery; the user must not be made to do the registration. Ordinary updates to existing mods reuse their original entries.

## Validation boundaries

2026-10-29 window layout fix: Release rebuild and 466 hidden-window checks passed, covering three window sizes, 100%/125%/150%/200% layout scales, eight- and twenty-item lists, fullscreen restore, and missing-file, unknown, running switches and start locks. A real window additionally checked scrolling to the last row, native maximize/restore, and F11/Esc round trips in normal and maximized windows — no actual mod toggling and no game start. Records and the reproduction entry are in `../research/evidence/mod-manager-window-20261029.md`; dragging between monitors with different DPI is unverified.

2026-10-24 Region Explorer integration: the status interface grew to 101 checks covering three mods' eight config combinations plus missing files, unknown states, and running locks; single-mod management grew to 54 checks covering the explorer's independent toggling, removal backup, and keeping the map/potion/data. The manager was rebuilt and its own drawing methods checked the three switch rows and window bounds — no game start and no user-window manipulation in that preview. The playtest copy's actual state was read-only verified with all three installed and enabled. In-game combination acceptance is still pending — see `../research/evidence/explorer-0.1.0-validation.md`.

The status interface passed 82 isolated checks, including default configs, independent switches, illegal configs, missing files, safety markers, read-only-no-write, and Windows PowerShell 5.1 Chinese JSON. The isolated process and shortcut scenarios use controlled simulated data; the real game directory's switch states are not modified in those tests.

2026-10-24 completed the visual check of a real Windows window and click verification on an isolated copy: all four switch combinations saved correctly, 4 toggles produced 4 backups, and 16 protected file hashes were unchanged; a wrong target locks and reading recovers after returning to the playtest copy. Details in `../research/evidence/mod-manager-validation.md`.

The same day, after adding "start game", the 82 status regressions and 25 start checks passed. The real window verified the button layout, click feedback, and the post-run lock; the start call on the isolated copy only logged the request, executing nothing — 18 protected files and the full mod config stayed unchanged. A real game start has not been measured.

A later review added `research/check_mod_manager_launch_process.ps1`: it launches a harmless test window through the manager's real call chain, confirming a timely return, a visible window, the correct working directory and process id, and no changes to the game/mod placeholder files. The user then started the playtest copy from the manager and confirmed it launches; the target process and launch log were verified read-only. The cause of one earlier non-response was never determined. In-game compatibility of all mod combinations is still not accepted.

When the current automation sandbox forbids CIM queries, the production status interface locks the switches normally; that must not be read as the game having exited, and it cannot be unlocked by ignoring the process checks. The real game copies have only been queried read-only — this validation changed none of their switches and performed no in-game compatibility acceptance.

## Fast Travel integration (2026-10-25)

Added the sixth entry, "旅行驿站" (Fast Travel), ID `FastTravel`, DLL `fast_travel.dll`, switch `fast_travel.enabled`. Name, description, install detection, unknown/missing/running locks, independent management, and the copy-preparation option were wired together. The original entry is kept, with the six rows scaled to the screen work area; the new row changed none of the other five states or upgraded their DLLs. Offline checks and the pending in-game items are in `../research/evidence/fast-travel-0.1.0-validation.md`.

0.1.1 added place renaming and multi-line notes; the manager keeps this entry, path, and switch — only the description was updated and the manager rebuilt, with no duplicate feature entries. Record in `../research/evidence/fast-travel-0.1.1-validation.md`.

0.1.2 (2026-10-26) added visited landmarks and confirmable boss name/class auto-registration, with personal notes kept separately; it reuses the same entry, path, and switch, syncing the status descriptions and UI copy and rebuilding the manager. Record in `../research/evidence/fast-travel-0.1.2-validation.md`.

0.1.3 (2026-10-26) fixed the missed F8 records for already-fought bosses; observation is independent of moving and casting and does not require every boss to have quest type 5; standing still is still what registers landing points. Keeping this entry and switch, the description was updated to "fights also record the boss; standing still registers the landing point", and the manager was rebuilt. Record in `../research/evidence/fast-travel-0.1.3-validation.md`.

## Boss Draw integration (2026-10-29)

The eighth entry, "首领抽奖" (Boss Draw), manager ID `BossDraw`, DLL `boss_draw.dll`, switch `boss_draw.enabled`. Independent status, install detection, missing/unknown/running locks, management, and copy-preparation entries were synced; the original double-click entry was rebuilt with eight items. F9 manual draw, vanilla loot preserved. The row now sits in the scrolling list with an equipment-card icon; the removed Map Light is not restored, and no other module config is changed. Offline validation and the pending real-game acceptance are in the Boss Draw delivery records.

Boss Draw 0.1.1 fixed the initialization rejection caused by the game's address relocation, reusing the same entry, DLL path, and switch; this DLL fix needed no manager rebuild. The playtest copy was updated as a backup; the user confirmed the F9 window opens; real kills, granting, and save/restart are still pending — see fix record (local validation records are not published with the source).

Boss Draw 0.1.2 reuses this entry, path, and switch, only updating the compensation-card description and, with authorization, appending one chance — no manager rebuild needed; it includes no other manager-window changes. See compensation record (local validation records are not published with the source).
