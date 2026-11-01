# Inventory Sort 0.1.1

Each category starts its own row with Chinese titles like chestplate, gloves, boots, pauldrons, and weapons. The sections grow and shrink with the actual item count; the cell size, cells per row, auto-wrap, and original tabs all follow the game's current layout — no fixed per-category capacity.

Updates the existing standalone `inventory_sort.dll`, reusing the manager's "背包分类" entry and `inventory_sort.enabled`. The display copy and the mouse position map to each other; the real inventory is not reordered, and prices, items, and saves are untouched; disabling restores the vanilla display. Titles and alignment blanks must not be treated as tradeable or droppable item cells.

After fully exiting the playtest game and launcher, run `Install-InventorySort.ps1` from the project root. The installer verifies the pinned Alpha game and shared framework, supports updating from verified 0.1.0, backs up the old DLL, full saves, and configs, keeps the other mods, and never starts the game. The formal copy needs separate explicit authorization.

This package contains only the standalone feature DLL and license, upgrading no shared framework. Build records and SHA256 are in `release.json`; the manager and install entry are in the project root. The real DLL, failure protections, and install checks passed; the layout mockup was checked; the real in-game display, trading, and combination compatibility are still pending acceptance. See the [feature notes](../../inventory_sort/README.md) and the validation record (local validation records are not published with the source).
