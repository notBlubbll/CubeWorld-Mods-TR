# Region Explorer 0.1.4

Standalone update of the existing Region Explorer mod: the place types "树木" (tree) became "巨树" (giant tree), "岩石" (rock) became "巨岩" (giant rock), and "陨石坑" (meteor crater) became "环形坑" (ring crater), bringing the Chinese names closer to the vanilla place meanings. The original game's generated proper names are unchanged.

Fixes some place proper names showing question marks due to missing glyphs. The explorer module uses its own font fallback to complete the display, replacing no original game word lists, shared loader fonts, or other mods' text, and writes no display fix into the character or world saves.

Keeps the vanilla difficulty colors, distance sorting, retained progress/completion states, paging, the cursor, and single-arrow navigation. Reuses the original manager entry, DLL path, and independent switch, bundling no updates to other mods or the shared framework. Map Light stays off.

After the game and launcher fully exit, install to the playtest copy via the root's `Install-RegionExplorer.ps1`. The script first backs up the full saves, the old DLL, and its own data, keeping the other files and switches; the formal copy needs separate explicit authorization. It never starts the game and does not support in-run replacement.

Basis and validation: `region_explorer/SOURCES.md`, `research/evidence/explorer-0.1.4-validation.md`. The build, glyph-coverage, DLL-combination and standalone runs, and update-protection checks are done, and it was installed into the playtest copy on 2026-10-25 12:28 after backup; software previews and offline checks do not equal real in-game acceptance.
