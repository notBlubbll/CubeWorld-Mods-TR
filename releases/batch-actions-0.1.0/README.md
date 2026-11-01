# Batch Actions 0.1.0 (playtest)

Empty bottles, water bottles, and health potions are handled in batches of at most 10, installed and toggled as a standalone mod.

## Usage

- **Buying empty bottles: hold Shift and right-click the empty bottle in the shop once.**
- **Making water bottles / health potions: hold Shift, click the vanilla craft button, and keep Shift held until this crafting round finishes.** The batch is processed after the vanilla progress completes, at most 10 units. Releasing Shift stops further batch calls; the vanilla unit already started still finishes the vanilla way.
- A normal click keeps the vanilla single unit; the vanilla long-press continuous crafting still works. Holding Shift and long-pressing craft launches batches continuously, at most 10 per batch.
- It stops when copper or materials run out, the vanilla carry limit for the item type is reached, or the menu, character, or selection changes. Each unit still charges the original price and consumes the original recipe's materials; the potion level and heal follow the currently selected native recipe, so higher recipes unlocked later work too.
- Only the empty-bottle purchase and the water bottle / ordinary health potion crafting are covered; other goods, equipment, buyback items, and other recipes keep the vanilla flow. Single-player only; batch is not enabled in multiplayer, while holding an item, or when the game is not in the foreground.

After fully exiting the game and launcher, run `Install-BatchActions.ps1` from the root; it installs to the playtest copy by default. The original "魔方世界模组管理器" (Cube World mod manager) has an independent "批量补给" (Batch Actions) switch. Installing never starts the game.

## Independent boundaries

| Item | Value |
| --- | --- |
| DLL | `mods/batch_actions.dll` |
| manifest ID | `local.cubeworld.batch-actions` |
| Manager ID / switch | `BatchActions` / `batch_actions.enabled` |
| Config and data | no new config keys or data files; the pre-existing `config/batch_actions.ini` and `data/batch_actions/` are kept |
| Shared dependencies | the verified Qube Loader and shared launcher; no dependency on the other feature DLLs |

`Manage-Mod.ps1 -Mod BatchActions -Action Disable/Enable/Uninstall` follows the existing management flow. Disabling keeps the DLL; removal moves only this DLL into the project backup. Saves and items already legitimately obtained or consumed keep the game's own saved results; disabling does not revert trades. Installing to the formal directory still needs the user's explicit authorization.

## Implementation and compatibility

The native purchase and craft entries are handled through the loader's generic function-interception interface; batch calls always complete synchronously on the game thread that started the operation — no background clicker and no direct writes to money or item counts. After each native call the output, charge, or material counts are verified; failures and unexpected changes stop immediately, with at most 10 calls per batch. Native crafting still completes one round of progress first; the batch part does not repeat the nine-progress wait per unit.

The purchase entry shares the loader's generic interception slot with Inventory Sort, registering only after all mods finish initializing, keeping Inventory Sort's initialization checks and click protection; the existing six mods' sources are unchanged. The craft entry cannot be occupied by another exclusive interceptor at the same time: the loader's on-demand `ITEM_PICKUP` capture also uses `0x4709c0`, and the current six mods do not subscribe to it. If that entry is already taken, this mod refuses to enable and records the reason — it does not override it. Future mods using that capture need separate verification; load order is not a compatibility guarantee.

Only the verified 2013 Alpha program, key instructions, and loader are accepted; unknown versions or foreign jumps refuse to enable. It verifies the inventory, the current recipe, and the requirement lists read-only, changing no prices, no native crafting-level algorithm, no inventory capacity, and no save format.

`Build-BatchActions.ps1` builds the standalone DLL and runs the batch-logic and real-DLL-init checks. The checks use synthetic memory and simulated native calls — they do not replace in-game trade, craft, save/restart, and combination acceptance. The native basis and reuse reasoning are in [SOURCES.md](SOURCES.md); this round's validation is in the record (local validation records are not published with the source).
