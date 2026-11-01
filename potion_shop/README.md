# Progression-Following Health Potion Shop 0.2.0

In single-player, the item shop (general store) sells the tier right below the best health potion the character can currently craft. Every time the shop opens, it re-reads the vanilla recipes this character has actually unlocked and keeps updating with the same algorithm — the tier is not hard-coded at some teen level.

## Tiers and prices

Vanilla health potion recipe levels are 1, 5, 9, 13, and so on. Let C be the highest recipe level the character has unlocked; the new goods are level C−4. For example, a level 13/14 character with the 13 recipe unlocked sees level-9 potions; after unlocking the 17 recipe, the shop next opens selling level 13.

When the highest recipe is level 1 there is no lower tier, so no health potion is added. On character-level errors, recipe-chain failures, inventory-capacity issues, or read-check failures it skips stocking — it does not guess levels. Reading recipes changes no character data; the other native goods and their order are kept.

The vanilla theoretical heal of an ordinary health potion is `H = 200 × 2^[3 × (1 − 1/(1 + 0.05 × (L−1)))]`. The price follows the gentle growth rule the user chose: `ceil(10 + (H−200)/40)` copper.

| Shop potion level L | Approx. theoretical heal | Price |
| ---: | ---: | ---: |
| 1 | 200 | 10 copper |
| 5 | 283 | 13 copper |
| 9 | 362 | 15 copper |
| 13 | 436 | 16 copper |
| 17 | 504 | 18 copper |
| 21 | 566 | 20 copper |
| 97 | 1118 | 33 copper |

Prices start at 10 copper and climb slowly along the vanilla heal curve; higher tiers expressible in the vanilla data are still computed by the formula, and prices never exceed 45 copper. The prices are a mod design choice and have not been balance-tested over long play.

The levels in the table are the internal levels of the vanilla items. During in-game acceptance, verify by the potion's actual heal and sale price; the tooltip does not need to display the same level number.

## Trading and independent management

- Still the same `mods/potion_shop.dll`, module ID `local.cubeworld.potion-shop`, and the manager's "Potion Shop" switch; no dependency on the map.
- One slot of the current-tier potion is appended at the end of row 0 of the merchant's temporary trade inventory; the merchant's entity inventory is not modified. No duplicate is added when the same tier of ordinary potion is already present.
- Display, balance checks, and charging share the price generated for this shop opening. Leveling up while the shop is open does not suddenly re-price the same item; the tier updates the next time the shop opens.
- Potion types, levels, and effects stay vanilla; no custom seeds. Potions already bought keep their original level; crafting, inventory items, and sale valuation still follow the vanilla rules.
- No direct writes to player money, health, or saves; the vanilla carry limit, crafting recipes, heal formula, and use time are unchanged. Structure padding bytes in the recipe are not used for item identification.
- Disabling keeps the DLL, config, data, and saves; removal moves only this DLL. Use `Manage-Mod.ps1` from the root or the manager, and only after fully exiting the target game and launcher.

## Build, update, and validation

The Win32 Release build targets are `potion_shop`, `potion_checks`, `potion_module_checks`. The candidate package is `releases/potion-shop-0.2.0`; the install entry is `Install-PotionShop.ps1` in the root, which by default touches only the playtest copy. The formal directory requires the user's explicit authorization for that target.

Before installing, it verifies the game, framework, candidate, and known old DLLs, fully backs up this copy's saves and the old potion DLL, and keeps the map, the other mods, the shared launcher, and all toggle keys; it never starts the game. Unknown DLLs, a running game, or an unverifiable process state all refuse to overwrite.

This round's offline checks covered the real DLL's recipe reads, leveling to 17/21/101, character switching, the one-tier-below selection, the shared-price condition for display and charging, read failures, and inventory preservation; these are not equivalent to real in-game acceptance. Results and install state are in `research/evidence/potion-shop-0.2.0-validation.md` and the candidate package's `release.json`.

Still to confirm in-game: that the shop tier matches the actually craftable tier, that buying one charges correctly, insufficient balance, stacking and carry limits, reopening the shop after leveling up, a normal save/restart, and each on/off combination with the map. The 0.1.0 Writes-declaration flaw was fixed in 0.1.1, and this version keeps that declaration.
