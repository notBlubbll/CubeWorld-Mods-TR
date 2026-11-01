# Inventory Sort 0.1.2

The section titles show only names like "胸甲" (chestplate), "手套" (gloves), or "武器" (weapons), dropping the cell counts next to them. The sections, vanilla cell size, per-row capacity, auto-wrap, and item handling follow 0.1.1.

Reuses the standalone `inventory_sort.dll`, the manager's "背包分类" entry, and `inventory_sort.enabled`, with no new entries or switches. After fully exiting the playtest game and launcher, run `Install-InventorySort.ps1` from the project root; the installer verifies versions and hashes, backs up the old DLL, full saves, and configs, updates only this module, and never starts the game. The formal directory needs separate explicit authorization.

Build info and hashes are in `release.json`; this round's validation is in the record (local validation records are not published with the source); the full notes are in [Inventory Sort](../../inventory_sort/README.md). The layout previews use illustrative icons; the real in-game display and operations are still pending acceptance.
