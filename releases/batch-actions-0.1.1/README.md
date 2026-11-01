# Batch Actions 0.1.1 (playtest)

Empty bottles, water bottles, and health potions get a visible quantity window. The default selection is 10; A / D decreases / increases by 10 each press; then confirm the purchase or crafting. Installed and toggled as a standalone mod.

## Usage

- **Buying empty bottles: click an empty bottle in the shop and the "choose quantity" window pops up.** Left, right, or middle click all work — no Shift needed.
- **Making water bottles / health potions: select the vanilla recipe and click craft.** After this round of the vanilla progress finishes, the "choose craft quantity" window pops up — at that moment no materials have been consumed and no potions produced.
- **A decreases 10, D increases 10, Enter confirms, Esc closes.** The window's +/- and confirm buttons work too; the minimum is 1 and it stops at the bounds. With no quantity available it shows 0 and locks the confirm button. Closing the window buys and crafts nothing.
- The window shows "owned / 50" and "max this round"; buying also shows the unit price, total price, and copper; crafting shows the materials on hand. The vanilla carries at most 50 of the same item; with 50 empty bottles you cannot buy more — fill bottles with water to consume them and free up the count. The max-this-round is limited by your balance, materials, and remaining carry space together.
- Each unit still charges the original price and consumes the original recipe's materials; the potion level and heal follow the currently selected native recipe, and the higher recipes unlocked later work too. After confirming it shows the actually completed count; on any change or call error it stops the remaining operations.
- Only the empty-bottle purchase and the water bottle / ordinary health potion crafting are covered; other goods, equipment, buyback items, and recipes keep the vanilla flow. Single-player only; in multiplayer or while holding an item, the vanilla flow stays. While the quantity is being chosen, the existing loader takes over the input — A / D do not move the character; losing focus, moving more than 1 cell, or a change of character or native menu closes the chooser.

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

The original purchase and craft-completion entries are captured through the loader's generic function-interception interface, waiting for the user's selection and confirmation first. Only in the confirmed frame event does it call the original game's trade and craft, verifying the same thread as the original operation; a different thread refuses processing. After every native call it re-checks the output, the charge, or the materials, and any failure or unexpected change stops immediately. Quantities never exceed the native carry cap of 50; there is no background clicker and no direct writes to money or item counts. Crafting runs one round of the vanilla progress first; the batch part does not repeat the wait per unit.

The quantity window uses the loader's unified input takeover and ImGui; the Chinese font reuses the independent font helper in the existing source and does not depend on the region explorer DLL. During input takeover the original shop hover may change; while buying, it only temporarily binds the captured shop row/column, clears the player-cell hover, and hides the equipment-interaction flag, going through the vanilla middle-click buy branch; after each unit it restores those UI fields and the input state. Money, inventory, and materials are still handled entirely by the original game; if the UI binding fails, no purchase is called.

The purchase entry shares the loader's generic interception slot with Inventory Sort, registering only after all mods finish initializing, keeping Inventory Sort's initialization checks and click protection; the existing six mods' sources are unchanged. The craft entry cannot be occupied by another exclusive interceptor at the same time: the loader's on-demand `ITEM_PICKUP` capture also uses `0x4709c0`, and the current six mods do not subscribe to it. If that entry is already taken, this mod refuses to enable and records the reason — it does not override it. Future mods using that capture need separate verification; load order is not a compatibility guarantee.

Only the verified 2013 Alpha program, key instructions, and loader are accepted; unknown versions or foreign jumps refuse to enable. It verifies the inventory, the current recipe, and the requirement lists read-only, changing no prices, no native crafting-level algorithm, no inventory capacity, and no save format.

`Build-BatchActions.ps1` builds the standalone DLL and runs the batch-logic, real-DLL-init, and production UI/callback path checks. The checks use synthetic memory and simulated native calls — they do not replace in-game trade, craft, save/restart, and combination acceptance. The native basis and reuse reasoning are in [SOURCES.md](SOURCES.md); this round's validation is in the record (local validation records are not published with the source).
