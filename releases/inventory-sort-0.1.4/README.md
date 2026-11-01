# Inventory Sort 0.1.4

Fixes the previous version's detail-yield coordinate error: the local bounds of the native details and the equipment comparison are converted to screen bounds through the node's current transform before clipping the section titles. Background, titles, and separators yield together; the titles restore when the detail closes. The original cell size, column count, wrapping, sections, and the names-only requirement are kept.

Reuses the original manager's "背包分类" entry, `inventory_sort.enabled`, and the standalone `inventory_sort.dll`. The root's `Install-InventorySort.ps1` requires a normal save and a full exit of the target game and launcher, verifying the known old versions and backing up the old DLL, full saves, and configs before updating. Playtest copy only by default; no other module or shared framework is changed, and the game is never started. The formal directory needs separate explicit authorization.

The real hover-coordinate replay, the DLL's synthetic checks, and the install protection scope are in the validation record (local validation records are not published with the source). The replay previews use illustrative icons and details — that does not mean the new version passed real in-game acceptance. One screen bound is logged on the first successful read of the main detail or the equipment comparison, for relaunch verification.
