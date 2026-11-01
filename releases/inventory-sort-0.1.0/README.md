# Inventory Sort 0.1.0 test package

Groups same-type items together in the original inventory, moving blanks to the end. The real item slots are not moved; vanilla equipping, taking/placing, and selling are kept. No grouping titles, fixed areas, or auto-selling were added.

The root's `Install-InventorySort.ps1` installs to the playtest copy by default and requires the game and launcher to be fully exited. It verifies hashes and fully backs up the saves, installs only this module, and keeps the original config, the other mods, and the shared framework. The manager name is "背包分类" (Inventory Sort) and it can be turned off independently.

`inventory_sort.dll` and `release.json` are this test build; the GPL-3.0 license is kept with the package. No game programs, assets, or saves are distributed. Output rebuilt from source goes to `build/bin` and does not auto-overwrite this release package.

For usage and acceptance boundaries see the [mod notes](../../inventory_sort/README.md) and the validation record (local validation records are not published with the source).
