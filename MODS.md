# Installing and Removing Mods Independently

2026-10-30: Boss Draw 0.1.7 updated in the playtest copy as an independent backup, fixing the receiver-object validation on death Worlds along the original program's call chain — which fixes 0.1.6 still rejecting real bosses. The original manager entry and independent switch are kept; other mods, the framework, saves, and records are preserved — see fix record (local evidence omitted from source publication).

2026-10-30: Boss Draw 0.1.6 updated in the playtest copy as an independent backup reusing the original entry and switch, fixing a missed precondition in cross-thread death observation. Saves, draw records, other mods, and the shared framework are preserved; real kills are still pending acceptance — see record (local evidence omitted from source publication).

2026-10-29: The five font updates were installed into the playtest copy using their own installers, DLLs, and switches, adding no dependencies and no new entries; the original font files, the loader, the original data, and the unselected mods are preserved — see record (local evidence omitted from source publication).

2026-10-29: Boss Draw 0.1.5 updated in the playtest copy reusing the original switch, unifying the window and the regular font weight, affecting only this mod; original data and saves preserved — see record (local evidence omitted from source publication).

2026-10-29: Boss Draw 0.1.4 updated in the playtest copy reusing the eighth switch, adding quality card faces, continuous rotation, and fixed info around the card; existing draw data and saves preserved, no manager rebuild needed — see record (local evidence omitted from source publication).

2026-10-29: Boss Draw 0.1.3 updated in the playtest copy reusing the eighth switch, adding only a lightweight card-flip display; the config, existing draw data, and the CWBOSS01 format are preserved, no manager rebuild needed. See animation record (local evidence omitted from source publication).

2026-10-29 New standalone feature: [Boss Draw](boss_draw/README.md). `boss_draw.dll`, `boss_draw.enabled`, manager ID `BossDraw`; F9 manual draw, original loot preserved. 0.1.1 fixed a relocation misjudgment and backed up the playtest update; the user confirmed the F9 window opens; real new-kill registration and prize payout are still pending acceptance, and old missed records were not backfilled automatically. The new feature and the other seven each toggle independently; `data/boss_draw/` is kept by default; Map Light remains removed per the original requirement. See this validation record (local evidence omitted from source publication).

The goal is that the map, potion shop, region explorer, map light, inventory sort, fast travel, batch actions, and boss draw can each be used alone, combined, or all disabled. They share the loader and have no required dependencies on each other.

| Part | Files | Responsibility and boundary |
| --- | --- | --- |
| Shared framework | `cube_mod.dll`, `NavigationLauncher.exe` | Loading and the launch entry; kept when a single feature is removed. The launcher name follows the existing entry. |
| Boss Draw | `mods/boss_draw.dll` | One extra draw per valid boss kill, F9 draw, a 40/40/20 base card pool with a fourth-draw class pity; single-player, saved independently — see this module's docs for details. |
| Batch Actions | `mods/batch_actions.dll` | After clicking an empty bottle or crafting water/health potions, pick a quantity; A / D decreases / increases by 10 each press, Enter confirms; each unit is charged at the original price with the original recipe costs. Toggle key `batch_actions.enabled`, no new data files. |
| Fast Travel | `mods/fast_travel.dll` | 0.1.3: nearby bosses that are moving or fighting are also observed automatically, keeping their name and class, and a teleport landing point is registered once they settle; F8 teleports, with manual registration, renaming, and notes. Single-player only; on a bad landing it attempts to return. Toggle key `fast_travel.enabled`, data in `data/fast_travel/`, no buildings generated. |
| Navigation | `mods/waypoint.dll` | Waypoints, navigation, town labels, and the shop guide; its own data in `data/waypoint/`. |
| Potion Shop | `mods/potion_shop.dll` | Sells the tier just below the best craftable health potion, updating stock and prices as you progress; does not depend on the map. |
| Region Explorer | `mods/region_explorer.dll` | 0.1.5 shows reliably-linked boss classes, limited spawn predictions, or unknown on the challenge page; keeps the giant tree/giant rock translated names, the extra labels, difficulty colors, retention states, and click-to-navigate — it is not a whole-world prediction or a complete history. Its own targets in `data/region_explorer/`. |
| Map Light | `mods/map_light.dll` | 0.1.1 lights the full M-map only at night; daytime stays vanilla. Toggle key `map_light.enabled`; it changes no time flow and generates no runtime data. The current playtest is still 0.1.0; the upgrade package installs after exit. |
| Inventory Sort | `mods/inventory_sort.dll` | Separate sections for chestplates, gloves, and so on, with Chinese section titles; the titles avoid item details and the equipment compare; taking and placing work, and empty slots are preserved after selling; keeps the vanilla slot size, per-row capacity, and auto-wrap. Toggle key `inventory_sort.enabled`; it does not reorder the real item slots. |

The eight features are built as separate DLLs with distinct module IDs. Region Explorer only reads already-generated map records; it writes nothing to the game, sets no reveal flags, and installs no game-logic hooks; it shares navigation targets with the map via optional messaging since map 0.3.6 and provides its own direction hints when used alone. Map compatibility updates go through `Install-TownLabels.ps1`, explorer installation through `Install-RegionExplorer.ps1`; they are not bundled over each other. The potion shop handles the shop inventory copy and prices; using it together with other shop- or price-changing mods still needs verification. Map Light is installed by `Install-MapLight.ps1`; 0.1.1 only temporarily replaces the native lighting function's input arguments while the M-map is open at night and passes through in daytime, writes no game memory directly, and bundles no shared-framework or other DLL updates; see [Map Light](map_light/README.md). Inventory Sort is installed independently by `Install-InventorySort.ps1`; it changes only the player inventory display and the mouse index mapping, not the real item slots, prices, or saves; see [Inventory Sort](inventory_sort/README.md).

Fast Travel is installed independently by `Install-FastTravel.ps1`; visits are recorded automatically, F8 returns to town or picks a landing point, and boss names/classes are saved only when verifiable on site; personal notes are kept separately, and only single-player worlds are supported — see [usage notes](fast_travel/README.md). You can manage it with `Manage-Mod.ps1 -Mod FastTravel -Action Disable/Enable/Uninstall`; preparing a new copy also supports `-Mods FastTravel`, leaving the default components unchanged.

Batch Actions is installed independently by `Install-BatchActions.ps1`, built and checked by `Build-BatchActions.ps1`. Clicking an empty bottle or crafting water/health potions opens a quantity window; A / D decreases / increases by 10, Enter confirms, Esc closes; it shows the vanilla carry limit of 50, and prices, materials, and the potion progression rules stay vanilla. Toggle "Batch Actions" in the manager, or use `Manage-Mod.ps1 -Mod BatchActions -Action Disable/Enable/Uninstall`; new copies support `-Mods BatchActions` with the original default selection unchanged. See [usage and compatibility notes](batch_actions/README.md).

## Managing the switches with the window

Double-click **魔方世界模组管理器.lnk** (Cube World mod manager) in the project root, or **启动模组管理器.cmd** (start the mod manager).

- The window can be resized by dragging its edges, and the title bar maximizes or restores. F11 or "Fullscreen" (top-right) toggles fullscreen, Esc exits it; the mod list scrolls independently, auto-splits into two columns in wide windows, and the text and switches do not shrink as mods are added.
- The playtest copy is shown by default; to operate on the formal copy, explicitly select "正式游玩" (formal play) in the window.
- The map, potion shop, region explorer, map light, inventory sort, fast travel, batch actions, and boss draw each have their own on/off switch. Clicking saves immediately and takes effect on the next game start.
- While the game or launcher has not exited, the directories mismatch, files are missing, or the process cannot be verified, the related switches lock with a reason shown.
- Turning a feature off keeps its DLL, waypoints, independent config, and saves; a backup is made before changes. "View backups" opens the backup directory.
- After setting the switches, click "启动游戏" (start game) at the bottom-right to enter the game through the shared launcher of the currently selected copy. It locks while the game is running or the state cannot be verified, to avoid double launches; opening the manager never starts the game.
- The window does not remove, reinstall, or update the shared framework. The start button reuses the installed, verified launcher and does not replace it along the way.

The interface uses a compact dark-blue list, thin-line controls, and the original blond adventurer icon. Only the main title can read the existing game font from the machine; the list and buttons use system Chinese fonts; the descriptions no longer pop up repeated hover tooltips. No game assets are bundled into the manager.

After changing the source, maintainers run `Build-ModManager.ps1` to regenerate the program and the local shortcut. The program lives in `build/bin/ModManager.exe`; the local shortcut and build outputs are not tracked by Git.

## Default delivery requirements for new mods

In any development conversation for this project, a new standalone mod must also be wired into the existing mod manager. The user only needs to describe the wanted feature; "hook it into the manager" does not need to be restated. This is a development and delivery requirement, completed by the agent responsible for that feature.

Each new mod's delivery includes at least:

1. A separate DLL, a unique ID, config and data boundaries, plus the necessary build and install entry points.
2. A Chinese display name in the manager, a short description, install status, and an independent switch. Lock explicitly when not installed, while the game is running, or when checks fail; never display an unknown state as off.
3. Wiring into the read-only status interface and the existing single-mod management flow, preserving the other mods' config keys, files, and saves; keep the original default behavior when adding copy-preparation options.
4. Rebuilding the manager so the user's existing double-click entry can manage the new feature, without making the user edit configs, run registrations, or open a second management tool.
5. The necessary independent toggle checks, running alongside existing mods, backup and data-preservation checks, and usage docs; anything not yet accepted in-game is explicitly listed as pending verification.

Without manager integration you may only report implementation progress, never declare the mod delivered. Only when the user explicitly asks for research-only, prototype-only, or deferred integration do you follow that narrower scope. Updating an existing mod should reuse its existing entry and re-verify compatibility, without duplicating switches for the same feature.

The current manager still works from the registered mod list; this project convention does not mean the program auto-detects arbitrary foreign DLLs. The developer of a new mod is responsible for registering it, wiring it up, and rebuilding on delivery; the user keeps using the same manager. Actually installing into the playtest or formal game directory still follows the authorization and backup requirements of the target.

## Preparing a new playtest copy

`Prepare-Playtest.ps1` by default prepares a copy with the map; you can also pick components with `-Mods`:

```powershell
# Prepare only the potion shop and the shared framework, no map DLL required
.\Prepare-Playtest.ps1 -Mods PotionShop

# Prepare two features at once
.\Prepare-Playtest.ps1 -Mods Navigation,PotionShop

# Prepare only the region explorer, or append it to the list
.\Prepare-Playtest.ps1 -Mods RegionExplorer

# Prepare only the map light and the shared framework
.\Prepare-Playtest.ps1 -Mods MapLight

# Prepare only inventory sort and the shared framework
.\Prepare-Playtest.ps1 -Mods InventorySort

# Prepare only the shared framework, install features later
.\Prepare-Playtest.ps1 -Mods @()
```

This only applies to a `playtest` that does not exist yet. Existing directories still refuse to be overwritten; it cannot be used to re-prepare a playtest copy currently in use.

## Operating a single mod

The following commands only target the playtest copy inside the project; fully exit that copy's game and launcher before running them. You can add `-WhatIf` at the end to only preview the planned actions.

```powershell
# Disable the map, keeping the map DLL, waypoints, and the other mods
.\Manage-Mod.ps1 -Mod Navigation -Action Disable

# Re-enable the map (requires the map DLL to still be present)
.\Manage-Mod.ps1 -Mod Navigation -Action Enable

# Disable or re-enable the potion shop alone
.\Manage-Mod.ps1 -Mod PotionShop -Action Disable
.\Manage-Mod.ps1 -Mod PotionShop -Action Enable

# Manage the region explorer alone; disabling keeps targets and the other mods
.\Manage-Mod.ps1 -Mod RegionExplorer -Action Disable
.\Manage-Mod.ps1 -Mod RegionExplorer -Action Enable
.\Manage-Mod.ps1 -Mod RegionExplorer -Action Uninstall

# Manage the map light alone; removal moves only the light DLL
.\Manage-Mod.ps1 -Mod MapLight -Action Disable
.\Manage-Mod.ps1 -Mod MapLight -Action Enable
.\Manage-Mod.ps1 -Mod MapLight -Action Uninstall

# Remove the potion shop: moves its DLL into backups and disables its startup entry
.\Manage-Mod.ps1 -Mod PotionShop -Action Uninstall

# Manage inventory sort alone; removal keeps the real inventory and all saves
.\Manage-Mod.ps1 -Mod InventorySort -Action Disable
.\Manage-Mod.ps1 -Mod InventorySort -Action Enable
.\Manage-Mod.ps1 -Mod InventorySort -Action Uninstall

# Before removing the map, the shared launcher that supports standalone mods must be in place
.\Manage-Mod.ps1 -Mod Navigation -Action Uninstall
```

Disabling only changes the selected module's `mods.ini` toggle key; removal additionally moves its own DLL into the project `backups/`. The other mods' switches, error counts, DLLs, and data are untouched. The selected mod's config and data, and all character and world saves, are kept by default.

If a module has been removed, restore its DLL through the matching install flow before re-enabling; `Enable` does not automatically restore code from unknown backups.

The scripts also offer an explicit `-Target Formal`, but it may only be used when the user explicitly asks to operate on the formal copy. It never starts the game.

## Transition between the old and new launchers

The old `NavigationLauncher.exe` checks that `waypoint.dll` exists. The new source removed that requirement, and the potion install scripts no longer require the map DLL.

The new launcher has only been compiled to `build/bin/NavigationLauncher.exe` and has not replaced the programs in the playtest or formal directories. So existing copies can first disable the map alone via `Disable`; until the shared launcher is upgraded and verified, the management scripts refuse to remove the map directly, to avoid breaking the potion shop's launch entry.

Upgrading the shared launcher and installing feature mods are two different operations. Updating the potion shop must not replace the shared launcher or the map DLL, and the shared framework must not be deleted to remove a feature.

## Compatibility and acceptance

- Inventory Sort only temporarily replaces the player's inventory display view; the other mods hook different points. The 32 switch combinations of the five features have been checked offline for status; hovering, taking and placing, equipping, consecutive selling, save/restart, and each combination in a real inventory still need in-game acceptance. See [Inventory Sort](inventory_sort/README.md).
- Splitting into separate DLLs reduces coupling but cannot guarantee that arbitrary mods never conflict; especially when two mods both change shop prices or the same set of goods, you must check how the rules combine.
- The management scripts were regression-tested for config changes, backups, and removal protection in isolated small directories; that does not mean every in-game behavior has been verified.
- Still to be accepted in-game: map only, potion shop only, both together, all disabled, and the other features starting after one is removed.
- Region Explorer additionally needs acceptance for running alone, sharing the arrow with the map, running alongside the potion shop, and the other features persisting after it is disabled. Eight configuration combinations have had offline status checks; those do not replace real in-game combination acceptance.
- Map Light additionally needs night-time acceptance: the M-map is brighter while open, and the night scene and time flow are normal after closing it — plus running alone, running with the other features, and behavior after disabling and removal. Sixteen switch combinations of the four features have offline status checks; the native lighting hook is at a different address from the potion hook, and real rendering compatibility is pending acceptance.
- The potion shop uses vanilla item types. Disabling or removing the shop is not a rollback of earlier trades; whether already-purchased items still work correctly must be confirmed in the playtest copy.
- Git stores code versions; development isolation across conversations or Git worktrees does not substitute for runtime mod compatibility design.

2026-10-23 offline validation: the launcher Release build, script syntax checks, 5 copy-preparation scenarios, and 41 single-mod management regressions passed. The management regressions ran only in an isolated directory under the project `build/`; due to CIM access limits in the current sandbox, the process branch was covered with a simulated manifest, and the real process list was additionally checked to confirm no game was running for the isolated copy. No install/removal was performed on the real playtest or formal directories, and in-game combination acceptance was not completed.

Boss Draw is installed independently by `Install-BossDraw.ps1`, built and checked by `Build-BossDraw.ps1`. `Manage-Mod.ps1 -Mod BossDraw -Action Disable/Enable/Uninstall` manages the single mod; preparing a new copy can use `-Mods BossDraw` without overwriting the existing playtest directory. Removal moves only this DLL into backups; the draw records and equipment already legitimately obtained are preserved.
