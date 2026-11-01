# Potion Shop fix pending-test version 0.1.1

2026-10-23 16:31 +08:00 — the playtest copy was updated, with the original mod and full saves backed up. The 115 inventory/pricing checks and the 24 real-DLL simulation regressions passed; the in-game retest is waiting for the user.

Fixes 0.1.0 loading but no potion appearing in the shop. The vanilla runtime logs confirmed: the inventory-copy hook did fire, but the mod had not declared `Writes`, so the framework restored the inventory parameters — and would also restore the price override. 0.1.1 adds that declaration; it is the mod's own capability declaration, changing no Windows or framework permission settings.

The feature is unchanged: 2013 Alpha single-player, the item shop sells the vanilla basic health potion at 10 copper a bottle; the other goods, higher potions, usage, and sale valuation stay vanilla.

Load through the playtest copy's existing "导航启动" (navigation launch). The install script `Install-PotionShop.ps1` requires the target game to be closed, backs up the full target saves and the old potion DLL, then updates only the potion mod. It updates no navigation and replaces no framework. The formal copy does not have this mod installed.

Still to confirm in the real game: after restarting, enter the item shop and see the potion at the end of the list, priced 10 copper; buying one gives +1 potion and −10 money; reopening the shop still allows buying; a normal save and restart keeps the counts and money right. Version and validation status are in `release.json`.
