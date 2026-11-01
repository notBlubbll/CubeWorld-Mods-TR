# Town Shop Labels 0.3.0 candidate

Compiled, not yet installed into the game currently being played. The target game must be exited normally before updating.

## New features

- Open the world map and nearby discovered shops and workshops are automatically named.
- Recognizes the inn, item shop, weapon shop, armor shop, identifier's shop, smithy, workshop, and clothier.
- Recognizes the furnace, anvil, spinning wheel, loom, saw, workbench, and customization bench.
- A "town facilities" list on the map's right shows the nearest facility of each nearby type and its distance.
- Right-click a shop name on the map, or a row in the facility list, to navigate directly; right-clicking elsewhere on the map still places free waypoints.
- Discovered facilities are saved per world; the existing navigation waypoints are kept.

The mod can only read and remember a town's shops after you first walk close to the town and open the map. Undiscovered distant towns do not get shop names out of thin air. When zoomed out, shop names take priority and dense facility text yields; zoom in or use the right-side list.

What is shown is the facility category like "weapon shop" or "smithy" — not the shopkeeper's name or an unverified proper shop name. It uses fixed signs and facility positions and does not track walking residents.

## Update and rollback

`Install-TownLabels.ps1` in the project root updates the playtest version; `-Target Formal` is for the formal one. The script checks the game has exited, verifies versions, backs up the old mod and waypoints, replaces only `mods/waypoint.dll`, and never starts the game. Both current game copies are still 0.2.0; this script has not been run.

After updating, keep launching from the original navigation entry. To roll back, exit the game normally and put this script's recorded backup `waypoint.dll` back into `mods`; the character saves do not need restoring. Waypoints and shop records are stored separately in the mod's data directory.

## Verified and pending

Passed the 51 original navigation checks and the 28 town-label checks, plus standalone image previews and list hit-tests for the regular window, the small window, and the no-facilities state.

Sign coordinates in a real town, alignment with buildings after map zoom, right-click navigation, restoration after exit and re-entry, and running performance still need post-install testing. The current read-only sampling saw no shop signs, so a real-town test cannot be claimed.

The preview images use illustrative data, not game screenshots. The font is read from the local game's existing files and not distributed with this candidate package.
