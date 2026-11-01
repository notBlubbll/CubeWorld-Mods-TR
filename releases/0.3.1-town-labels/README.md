# Shop Sign Names 0.3.1

Only adds Chinese names to the shop signs the game already draws. Supports the inn, item shop, weapon shop, armor shop, identifier's shop, smithy, workshop, and clothier. Furniture, crafting benches, ordinary residents, and decorations show no shop names.

Launch via the development copy's original entry `C:\Projects\CubeWorld-Mods\playtest\NavigationLauncher.exe`, pick the original character and world, and press M for the map. Right-click a shop name or a name in the right-side list to navigate; the rest of the map keeps free waypoints.

0.3.0 had wrongly used indices from the structure-model table as furniture types, labeling shelves as inns or item shops. 0.3.1 instead reads the entities the original game draws signs for — category 3, roles 128–135; the old facility cache auto-invalidates and rediscovers, and the existing destination waypoints are kept.

Only loaded shop signs are read; far-away regions are not force-loaded. Towns not yet approached may have no names. The list shows the nearest shop of each type; the map can still mark multiple real shops of the same category.

Install with the project root's `Install-TownLabels.ps1 -Target Experiment`; the target game must be exited normally first. The script verifies the game and framework versions, backs up the old mod and mod data, replaces only the navigation DLL, and verifies the original program, config, and saves were not rewritten by the installer. The formal directory requires an explicit `-Target Formal`.

The source is in the project's `navigation/`; the framework license is kept with the package; the font is read from the game's original directory and not distributed with the mod. Build hashes and check results are in `release.json`.
