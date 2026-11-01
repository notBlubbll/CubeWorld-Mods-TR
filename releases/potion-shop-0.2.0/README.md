# Potion Shop 0.2.0 pending-test version

The user's decision: the shop's health potion is always one tier below the character's best craftable, with gently rising prices. Each shop opening reads the recipes this character has actually unlocked; at level 13/14 with the level-13 potion unlocked, it sells the level-9 potion for 15 copper. After unlocking level 17, the shop next opens selling the level-13 potion for 16 copper; higher tiers keep being computed by the same formula.

- With only the basic recipe there is no lower tier, so no potion is added.
- The vanilla effects, materials, carry, and usage rules continue; potions already in the bag keep their original level.
- Reuses the standalone `potion_shop.dll`, the module ID, and the manager's "Potion Shop" switch.
- Replaces no map, shared framework, or launcher, and resets no toggle keys or other mods' data.

Build targets: `potion_shop`, `potion_checks`, `potion_module_checks`, Win32 Release. The candidate DLL hash, build, and install state are defined by the sibling `release.json`; the check records are in `../../research/evidence/potion-shop-0.2.0-validation.md`.

Run `Install-PotionShop.ps1 -Target Experiment` from the project root to update the playtest copy. The install must verify the game and launcher fully exited and back up the full saves and the old potion DLL; the formal copy needs the user's separate explicit authorization. The script never starts the game.

In-game stocking, purchase charging, stacking, the level-up update, save/restart, and the map combination compatibility are still pending acceptance. Offline checks and the actual load do not replace them.
