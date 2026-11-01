# Inventory Sort 0.1.5

Fixes the equipment page falling back to the vanilla mixed layout when the data crosses an adjacent readable memory region. When a whole-block read fails, it re-reads in segments split at Windows page boundaries through the original loader's verified reads; any unreadable segment still stops the sectioning. Restores the Chinese sections for chestplates, gloves, boots, pauldrons, weapons, amulets, and rings, keeping the original cell size, column count, wrapping, and item handling, with titles showing only the category names. The 0.1.4 detail-yield fix is kept.

Reuses the original manager's "背包分类" entry, `inventory_sort.enabled`, and the standalone `inventory_sort.dll`. The root's `Install-InventorySort.ps1` requires a normal save and a full exit of the target game and launcher, verifying the known old versions and backing up the old DLL, full saves, and configs before updating. Playtest copy only by default; the game is never started; the formal directory needs separate explicit authorization.

Replayed with the live 104-item equipment's full bytes and memory bounds, checking the sections, hover, selection, stale-position protection, and the unreadable tail segment. The validation and real-game state are in the record (local validation records are not published with the source).
