# Inventory Sort 0.1.6

Fixes the original position collapsing after picking an item up onto the mouse, and visible blanks not accepting the put-back. After a displayed item is picked up or sold, the empty slot is kept in its section; other items and later sections do not shift forward. While holding an item of the same tab, the kept slots and the visible row-end blanks receive the put-back through real native slots, and the title rows stay unclickable. The original cell size, wrapping, Chinese category names, the detail yielding, and the cross-region read fix are kept.

Positions persist within the current game session, including tab switches and closing/reopening the inventory. After restarting the game or switching characters, the sections are rebuilt from the actual items; no new position save file. When new items need more rows, the original column count auto-extends as before.

Reuses the original manager's "背包分类" entry, `inventory_sort.enabled`, and the standalone `inventory_sort.dll`. After a normal save and a full exit of the target game and launcher, the root's `Install-InventorySort.ps1` verifies the known old versions, backs up the old DLL, full saves, and configs, then updates only this module; playtest copy by default, never starting the game. The formal target needs separate explicit authorization.

Replayed with real 105-cell records from before and after a pickup — the in-place put-back, the visible-blank put-back, and the post-sale positions; the take/place in the real-DLL simulation uses controlled native stand-ins. The validation and real-game state are in the record (local validation records are not published with the source).
