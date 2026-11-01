# Region Explorer 0.1.6 (boss-class test version)

This font tweak: keeps the original FangZheng CuYuan glyph shapes, improves pixel alignment and sampling, and lightens the black outline on the map/explorer panels; no layout or gameplay changes. The playtest copy was updated as an independent backup; the real look is still pending a re-launch to confirm. See font record (local validation records are not published with the source).

Lists the nearby towns, challenges, and landmarks the game has already generated and still holds in memory, on the left side of the vanilla M map, sorted by straight-line horizontal distance. Left-click a row to set it as the destination, close the map, and keep going.

0.1.5 adds boss class info under each record on the "Challenges" page:

- **Confirmed**: after the spawn record, place, current quest, and entity number all match, the actual boss's class is read — the camera does not need to see it.
- **Predicted**: only for regular bosses of the current quest type 5 that already have detailed spawn records, computed independently from the original program's quest random seed and entity number. It is not guessing a class, and places are not filtered by the player's class; it still needs in-game comparison against the real result.
- **Boss class unknown**: kept unknown — and the place is not hidden — when there is no detailed record yet, another spawn branch applies, linking fails, or several bosses cannot be ruled out. Special species show "special boss" once verified; finished places with no historical class show "boss class not recorded".

This is not a seed prediction of the whole infinite map. The regular class prediction covers only resident detailed spawn records; farther coarse-map landmarks may still show unknown. It refreshes every two seconds, and only the "Challenges" page of the M map queries up to a 7×7 grid of detailed chunks around the player — at most 8192 spawn records total, 4096 per chunk; no such query runs with the map closed, on other tabs, or while walking. It reads only the current spawn records and entities — no forced chunk loads, no monster spawns, no calls to the game's random functions. The vanilla level colors, place categories, distance sorting, retained completion states, fonts, and navigation are kept. If a prediction disagrees with the actual boss of the same number, prediction stops for this world session and the verifiable actual read stays. In-code combination and error-path checks exist; the real in-game class and frame rate are still pending acceptance.

0.1.4 re-checked the vanilla types against Alpha references: "tree" became "巨树" (giant tree) and "rock" became "巨岩" (giant rock); `Crater` does not prove a meteor origin, so it became "环形坑" (ring crater). The other types are kept. All 40 place-name syllables were strictly verified against the local Chinese DLL, with no literal question marks in the word lists; the "露?龙" in the screenshot was really "露芶龙", but the original font lacks "芶" (U+82B6). The explorer uses an independent font object, preferring the original game's glyphs and filling only missing characters from the local system font; the original proper names, the shared default font, the other mods, the shared framework, and the saves are untouched. The new font is built once on first need and reused — no per-frame font-file reads; the old backend without dynamic fonts stays as-is and is logged unavailable.

0.1.3 adds the vanilla place type to names, for example "宫殿 · 瓦丽雅" (Palace) or "金字塔 · 泰勒莫" (Pyramid); the proper names still come from the runtime word lists — none were invented. Castle, ruins, catacombs, palace, temple, and pyramid — all category 14 — are told apart by subtype; unverified combinations just say "landmark". This is a place type, not a biome name like desert or snow, and not a boundary prediction.

"Challenges" lists the current quests plus nearby records that still have progress/completion state. The old version looked only at the quest number and missed places whose number was cleared but whose state was still 1/2; those are now labeled "retained progress/retained completed", distinct from current quests. The top shows the current tab's record and completion counts, and the pages are computed automatically; unfinished first, completed after. It is not a statistic of the character's whole kill/clear history, and it does not assume every forest or building has a boss. After leaving the load range, a game reset, or a state clear, old records may no longer be listed; this version builds no permanent history archive.

0.1.2 adopts the vanilla landmark level colors in the landmark and challenge lists: **white lower, cyan comparable, red higher**. Even for far places, as long as the region level is readable from the generated record, it shows early. Each row keeps the quest state, difficulty text, and distance; the selection uses a gold frame, and completed keeps the green status text — the difficulty color of the name is never overridden. Towns stay white with no combat-difficulty recommendation.

This is the vanilla comparison of region level to the current character level; it does not evaluate equipment, class, monster count, or skill, and it does not promise a cyan place is beatable. It is not a fixed level-difference threshold; the algorithm and original bytes are in [SOURCES.md](SOURCES.md). On a character-level read failure or an abnormal value the name grays out with "difficulty unknown" — no distance-based color guessing. Re-comparison is live after leveling up.

0.1.4 keeps the existing "区域探索" (Region Explorer) manager entry and independent switch, updating only the explorer DLL; the coarse-map scan range, two-second refresh, distance sorting, map waypoints, and storage format are unchanged, with no extra map generation and no game hooks. 0.1.5 only adds the on-demand boss query above. Map Light keeps the user's off state. The latest install and acceptance status is in `research/evidence/explorer-0.1.5-validation.md`.

0.1.1 fixed the invisible cursor over the explorer panel: the native cursor was drawn before the mod panel and got covered; now a white pixel hand is drawn last, only inside the explorer panel and with the game in the foreground. The fingertip and the list click test use the same coordinates; on closing the map or leaving the panel the original game shows the cursor again — no global cursor-state changes.

This version reads existing map data; it does not generate far-away maps from the seed and has no full region-boundary prediction. Not appearing in the list does not mean a place does not exist. Towns can be recognized earlier than shopkeeper entities; challenges are listed by the original record's quest number or a non-zero state — not every landmark can be called a boss. The original map save holds no live quest state, so no complete clear history can be fabricated from an ordinary map save.

## Usage

1. Installs to the `playtest` copy by default; the formal copy is never auto-installed. After the game and launcher fully exit, run `Install-RegionExplorer.ps1` from the root.
2. Turn on "区域探索" (Region Explorer) in the existing mod manager and start via the original playtest entry yourself. Do not install or hot-plug into a running game.
3. After entering a character and world press M and pick "Towns", "Challenges", or "Landmarks"; the "Challenges" page shows the boss class info. Left-click a place in the list to set the target — it reads once every two seconds, and paging shows more places.
4. With Navigation 0.3.6 also enabled, they share the original top direction bar and one down arrow; no extra map nameplates. Map compatibility updates go through `Install-TownLabels.ps1` independently and are not bundled by this installer.
5. With Navigation disabled or absent, this module shows its own bottom-right direction/distance hint and single map arrow. With the old Navigation installed, first update the map to 0.3.6 alone, then enable both, to avoid the old navigation leaving a manual target behind too.
6. "Cancel explorer target" at the list bottom clears only this feature's target. When running together with the new map, a later manual marker wins; the explorer target is not re-seized every frame.

Place names are read from the original program's runtime generation word lists; local Chinese-game screenshots already show Chinese place names, and other localization combos still need confirmation. No translation was invented. Numbers use Navigation's game coordinate unit `m`, not yet calibrated to real meters. Targets are the place center's XY — not guaranteed to be a building entrance or the boss entity's exact spot, and no obstacle pathing is provided.

## Independent boundaries

| Item | Value |
| --- | --- |
| DLL | `mods/region_explorer.dll` |
| manifest ID | `local.cubeworld.region_explorer` |
| Manager ID | `RegionExplorer`, Chinese name "区域探索" |
| Switch | `region_explorer.enabled`, in the shared `mods.ini`; the other keys are untouched |
| Data | `data/region_explorer/world_<seed>_<namehash>/target.bin` |
| Capabilities | Overlay, RawMem only; no Writes or RawHooks declared |
| Read limits | coarse map ±3 storage blocks around the player, 64 places per block; only the "Challenges" page additionally queries up to 49 detailed chunks nearby — at most 8192 spawn records total |

Use the existing `Manage-Mod.ps1 -Mod RegionExplorer -Action Disable/Enable/Uninstall`. Disabling keeps the DLL; removal moves this DLL into the project backup, keeping the data and all saves. Map, potion, and explorer each toggle independently. The existing old shared launcher still requires the map DLL file; the map can be disabled on its own, and removing the map directly stays locked by the original management flow — this module cannot bypass it.

## Build and checks

Run `Build-RegionExplorer.ps1` from the root — build and checks only, no install, no game start. `research/inspect_explorer.py --saved` checks the playtest map save read-only in SQLite mode; `--live` reads only the verified game-process path. Sources and offset basis in [SOURCES.md](SOURCES.md). Current evidence and the remaining in-game acceptance are in `research/evidence/explorer-0.1.0-validation.md`.
