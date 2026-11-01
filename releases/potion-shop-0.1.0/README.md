# Potion Shop pending-test version 0.1.0

**Known issue: missing Writes declaration — the runtime stocking and price changes were reverted by the framework. This package is kept for reproduction and is no longer used for new installs; see the sibling `potion-shop-0.1.1` for the fixed version.**

Buy basic health potions at the 2013 Alpha's vanilla item shop (general store), 10 copper a bottle, single-player only. Higher-tier potions are not part of this pricing. Healing, use, stacking, and carry limits stay vanilla; crafted potions and sale prices stay vanilla.

Load through the existing "导航启动" (navigation launch); this is the standalone `potion_shop.dll`, working alongside the navigation mod. No replacement of `Cube.exe` or the mod framework. The attached files are the mod, a version/hash manifest, and the GPL-3.0 license; the full source is in the project's `potion_shop/`.

The first Win32 Release build compiled successfully, and the 115 offline inventory and pricing checks passed. The in-game price display, purchase charging, stacking, and save/restart are still to be verified.

The install script is the project root's `Install-PotionShop.ps1`, targeting `Experiment` by default (`C:\Projects\CubeWorld-Mods\playtest`). The script requires the target game to have exited, verifies the game and framework versions, backs up the full target saves, then adds only this mod. `-ValidateOnly` checks without installing; the formal directory needs an explicit `-Target Formal`.

When trying it out, enter the item shop: confirm the new health potion is priced at 10 copper; note your money — buying 1 should cost 10 copper and add 1 potion; same-stat potions should merge into a stack; purchases should be refused when money is insufficient or the vanilla carry limit is reached. Close and reopen the shop — the potion should still be purchasable. Then exit normally and relaunch to verify the money and the potion count.

To roll back, exit the game normally and move that copy's `mods\potion_shop.dll` out of the `mods` folder. The bought potions remain vanilla items; no save restore is needed to uninstall the mod.
