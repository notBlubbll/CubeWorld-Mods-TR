# Inventory Sort 0.1.3

Fixes the section titles covering scroll details, item stats, and the equipment comparison. The titles are clipped against the actual bounds of the two native detail nodes, keeping the detail text and borders; the other sections keep drawing, and it restores when the detail closes. When the bounds cannot be verified, the titles hide for that frame. The cell size, wrapping, sections, mouse behavior, and the names-only display follow the original behavior.

Reuses the original manager's "背包分类" entry, `inventory_sort.enabled`, and the standalone `inventory_sort.dll`. Run `Install-InventorySort.ps1` from the project root — it requires a normal save and a full exit of the playtest game and launcher; it precisely verifies the known old versions, backs up the old DLL, full saves, and configs, then updates, replacing no other mods, the shared framework, or the launcher, and never starting the game. The formal directory needs separate explicit authorization.

Builds and hashes are in `release.json`; the validation scope is in the record (local validation records are not published with the source). The software previews use illustrative details and icons; the real in-game fix effect and operations are still pending acceptance.
