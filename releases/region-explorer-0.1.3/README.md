# Region Explorer 0.1.3

Standalone update of the existing Region Explorer mod: the names gain the vanilla place type, accurately telling apart forests, palaces, pyramids, and the like; the challenge list brings back records with no current quest number but retained progress/completion state, clearly labeled "retained progress/retained completed". The panel shows the current tab's counts and explains that it covers only nearby loaded records — not a complete clear history.

Keeps the vanilla difficulty colors, distance sorting, paging, the cursor, and single-arrow navigation. The original manager entry, DLL path, and independent switch are reused; no other mod or shared framework is updated, and the potion, inventory, and saves are untouched. Map Light stays off.

After the game and launcher fully exit, install to the playtest copy via the root's `Install-RegionExplorer.ps1`. The script first backs up the full saves, the old DLL, and its own data, keeping the other files and switches; the formal copy needs separate explicit authorization. It never starts the game and does not support in-run replacement.

Basis and validation: `region_explorer/SOURCES.md`, `research/evidence/explorer-0.1.3-validation.md`. Software previews and offline checks do not equal real in-game acceptance.
