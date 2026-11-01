# Inventory Sort 0.1.7

This font tweak: keeps the original FangZheng CuYuan glyph shapes, improves pixel alignment and sampling, and lightens the black outline on the map/explorer panels; no layout or gameplay changes. The playtest copy was updated as an independent backup; the real look is still pending a re-launch to confirm. See font record (local validation records are not published with the source).

Sections the original inventory by category — chestplates, gloves, boots, pauldrons, weapons, amulets, rings, and so on. Each category starts its own row with a Chinese title. Per the user's latest requirement, when displayed equipment is sold or picked up the empty slot is kept — later equipment and categories do not shift forward to fill it; when new items need more rows, the original column count extends as-is. Within a category the relative order of the real inventory is kept; there is no extra sorting by level, quality, or price.

Cell size, cells per row, visible rows, and auto-wrap all read the current vanilla layout — nothing is fixed at 10 cells, 10 rows, or a per-category capacity. The vanilla tabs, item icons, stat tooltips, and mouse behavior are kept. Scrolls, recipes, consumables, and materials are grouped by their native types and keep the tabs they originally belonged to.

The titles show only the Chinese category names like "胸甲" (chestplate) or "手套" (gloves) — no cell counts or quantities; an item's own stack count is still displayed by vanilla. A title takes a row of display space by itself; the blank alignment at a section's end must not be taken as added inventory capacity. When sections are first built, unused native empty slots are not padded into the section area; positions that were displayed keep their empty slot after a later sale or pickup. While holding an item from the same native tab, you can place it back directly into the kept slot or the visible row-end blanks — the only entry is no longer at the list's end. Blank-cell actions still point to actually existing native slots; the real inventory is not expanded.

Position records live only within the current game session; switching tabs or closing and reopening the inventory keeps them. Restarting the game or switching characters rebuilds the sections from the actual items; no new position save file is added.

When hovering a scroll or equipment, or showing item details and the currently-equipped comparison, the section titles convert the native window's local bounds to screen bounds through the node's current transform and yield — no background, text, or separator is drawn in the overlap; the other sections keep drawing; it restores when the detail closes. Yielding only changes the draw clipping — cell positions and scrolling do not jump. If a visible detail's bounds cannot be verified in a frame, the titles hide for that frame; readable details come first.

## Usage and independent management

By default this installs into the project's `playtest` copy. After fully exiting the game and launcher, run `Install-InventorySort.ps1` from the root, or have the developer do the install; then turn on "背包分类" (Inventory Sort) in the existing "魔方世界模组管理器" (Cube World mod manager), start the game yourself, and open the inventory. No new in-game hotkeys.

| Item | Value |
| --- | --- |
| DLL | `mods/inventory_sort.dll` |
| Module ID | `local.cubeworld.inventory_sort` |
| Manager ID / name | `InventorySort` / "背包分类" (Inventory Sort) |
| Independent switch | `inventory_sort.enabled`, in the original `mods.ini` |
| Runtime data | no new data files; the existing `data/inventory_sort/` and `config/inventory_sort.ini` are kept |
| Shared dependencies | the verified Qube Loader and the original shared launcher; no dependency on the other feature mods |

Use the original manager switch, or `Manage-Mod.ps1 -Mod InventorySort -Action Disable/Enable/Uninstall`. Disabling keeps the DLL; removal moves this DLL into backups. The next launch restores the native display; this module never reordered the real inventory. Installing, replacing, or removing while the game or launcher runs is not possible. The formal copy needs the user's explicit authorization for that target; installing never starts the game.

The existing old shared launcher still requires the map DLL file to exist; the map can be disabled on its own. The restriction on removing the map directly follows the root `MODS.md`; the inventory install does not bundle a shared-launcher upgrade.

## Implementation and protections

During drawing, the player inventory Widget temporarily reads a sectioned copy of the full item records; afterwards it restores the original pointer and maps the hover position back to the real item cell. The selection box also converts between display and real positions. Title rows have no real item mapping and cannot be sold, equipped, or receive drags. With nothing held, a blank cannot select another item; when holding an item of the same tab and a real empty slot exists, the kept slot and the row-end blanks can receive the native put-back. Display aliases always map to verified real slots, and the selection box follows the actual hover position. The scroll range is computed from the sectioned length; the actual drawing is still done by vanilla; the Chinese titles are drawn through the existing loader's draw interface. The merchant, recipe, and other Widgets keep their original behavior.

Item counts, seeds, stats, quality, stacking, prices, the real inventory order, and character saves are never written by this module. SDK write permission is used only for three fields: the UI's item-list pointer, hover cell, and selected cell. The copper and platinum coins that the native render reads at the same time are also copied into the display view, to avoid an out-of-bounds read from swapping in a short vector.

0.1.6 fixed the empty slot vanishing after a pickup and the inability to put back into visible blanks, and keeps sold slots per the latest requirement. The draw view and the post-trade scroll view share the section-position record isolated by controller, character, and inventory base address; the click snapshot is still independently verified. The held-item check compares the full native record — swapping in another item of the same count also clears the stale position.

0.1.5 fixed the equipment page falling back to the vanilla mixed layout when the data crossed an adjacent readable memory region. After a whole-block read fails, it re-reads in segments split at Windows 4 KiB page boundaries through the same SDK; each segment must still pass the loader's readability check, and any failing segment keeps the vanilla display. The read limit is unchanged; the shared framework was not upgraded or modified. On the first successful sectioning of each native page, it logs once — page number, original cell count, category count, and original column count — to distinguish whether a given page actually took effect; these numbers are log-only, and the in-game section titles show no counts.

If the inventory contents, character, tab, or held item change before a click, the old hover and selection positions are cleared and the action waits for the next draw — stale positions are never reused. The post-trade scroll recomputation uses an independent copy and never mistakes not-yet-displayed new positions for displayed ones. An exceptional exit from drawing also restores the pointers. Unknown versions, a hook at the same address, or an unavailable title interface refuse integration; a layout read failure, abnormal item records, or exceeding the read limit keep the vanilla display.

## Build and validation

`Build-InventorySort.ps1` builds the standalone DLL and runs the synthetic-SDK checks of the real DLL; it does not install or start the game. The release package is `releases/inventory-sort-0.1.6`; the installer verifies the package, game, and framework hashes, supports updating from verified 0.1.0 / 0.1.1 / 0.1.2 / 0.1.3 / 0.1.4 / 0.1.5, and backs up the old DLL, full saves, and configs while keeping the other mods, config keys, and data.

2026-10-25 the user relaunched and confirmed 0.1.6: "put-back works, and selling leaves no fill-in". The pickup/place and sale-position fixes are in the validation record (local validation records are not published with the source). 2026-10-25 the user had already relaunched and confirmed 0.1.5's equipment sections and the detail display working together. This round's equipment-page read fix, the real runs, and the live 104-item full-equipment replay are in the 0.1.5 record (local validation records are not published with the source). 0.1.3 still occluded on real hover: it had treated local coordinates as screen coordinates. 0.1.4 fixed the transform with real hover data — see the 0.1.4 record (local validation records are not published with the source); the earlier attempt is kept in the 0.1.3 record (local validation records are not published with the source), the removal of title cell counts in the 0.1.2 record (local validation records are not published with the source), the sectioning implementation and the pending real in-game acceptance in the 0.1.1 record (local validation records are not published with the source), and the native basis in [SOURCES.md](SOURCES.md). The software preview is a layout illustration; synthetic inputs do not mean real trades were executed in-game or that all compatibility acceptance is complete.
