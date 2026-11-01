# Fast Travel 0.1.4 (playtest)

This font tweak: keeps the original FangZheng CuYuan glyph shapes, improves pixel alignment and sampling, and lightens the black outline on the map/explorer panels; no layout or gameplay changes. The playtest copy was updated as an independent backup; the real look is still pending a re-launch to confirm. See font record (local validation records are not published with the source).

Cuts the time spent returning to towns for resupply and selling, and re-running long routes. It follows the visit records, merchant landing points, and return-to-nearest-town flow of [Trader Fast Travel](https://github.com/synap5e/cubeworld-trader-fast-travel), adding basic info for visited landmarks plus confirmable auto-registered boss names and classes. Then press **F8** to pick a place to teleport to, or "return to the nearest town". Other wild locations can also be registered manually; it generates no buildings or NPCs and changes no town count or world generation.

## Automatic visit registration

0.1.3 fixes "seen it, fought it, still no F8 record": boss observation runs at most once per second, independent of the moving, casting, jumping, and standing-still requirements. Within 512 cells horizontally and 256 vertically nearby, once a reliably-linked real boss is read, the name and class are remembered immediately; if the place already has a station, it just fills in — the landing point and the handwritten content are untouched. It does not require quest type 5 to exist, nor the F8 window to be open. The distance range is a bounded read condition, not camera visibility or an unobstructed line of sight.

The observation links a spawn record to its spawn location via the entity's full ID, species, level, and the giant-boss flag; special boss species follow the verified SDK list; quest type-5 quest bosses still get their detailed quest identity checked. When several confirmable bosses share a location, the one the original game's crosshair clearly targets is recorded first; without a clear target, no nearest one is picked arbitrarily. A still-resident downed boss can also be read; historical bosses that were never read and have been removed by the game cannot be fabricated.

Boss info is saved separately in `encounters.bin`, isolated by character and world, keeping the latest boss observation for at most 32 places; it contains no directly teleportable landing points. It survives normal exits, entity unloads, and restarts. For existing stations, the auto info — plus manual records that are unlinked but whose landing point belongs to that native place cell — gets the boss info appended; custom names, notes, and the underfoot landing point are kept. An observation record is not a kill verdict or a complete boss profile.

Teleport landing points still require dismounting and standing still for about 3–6 seconds: within 256 cells of a landmark center, or inside a native place cell that already has a boss observation, the player's actual underfoot landing point is saved. The registration path of within 96 cells horizontally / 32 vertically of an original quest boss and 1024 cells of the landmark center is kept. Towns keep using the standing spot next to the shops; while moving or fighting, only info is recorded — no aerial or moving teleport points are created. Without a reliable link it stays unknown; special bosses are not force-fitted with ordinary classes.

The auto info is labeled "last on-site record". A new quest re-verifies; cleared quests or temporary unloads keep the history; the basic info and the personal text are saved separately. An auto-created landing point prefers to fill in the record for the same place, or an unlinked manual record within 32/4 cells; when the 32 are full, old records are not pushed out. The old 0.1.2 behavior of only querying quest type 5 after standing still is superseded by this observation mechanism.

## Editing names and notes

F8 → select a registered place → **edit name/note**. The name can become whatever is easy for you to recognize; the multi-line note can record difficulty, tactics, whether you have beaten it, and your next plans. The auto info on top is for reading; the text below is yours to write. Click "save changes" to store it; "cancel edit" discards the draft. Town records can be edited too; the original town identity and landing point are kept.

The list shows the boss info and the note's first line; after selecting, the lower area scrolls through the auto info and the full note; the edit page can view and change the full note. It does not guess tactics, difficulty impressions, or whether you have won. Names cannot duplicate or be empty, keeping the 64-byte cap; notes are at most 1024 bytes and may wrap.

Old `CWTRAVEL1/2/3/4` places load directly; unobserved bosses are not fabricated and old records are not batch-rewritten; `CWTRAVEL4` is still used. The observation history also uses the bounded encoding; spawn positions are only used for info linking and never enter the teleport list. The per-file cap is 128 KiB; before saving it verifies, keeps, and reads back `stations_backup.bin` or `encounters_backup.bin`, and a write failure attempts recovery. An observation-history failure keeps the live info and retries; a corrupted history is kept and overwrite-locked, without locking the other healthy stations. The installer additionally backs up the full data; rolling back to 0.1.2 can keep reading existing stations, and the independent history file is kept.

## Usage

1. After fully exiting the game and launcher, run `Install-FastTravel.ps1` from the root; it installs to the playtest copy by default and updates no other mod or the shared framework. The original mod manager has an independent "旅行驿站" (Fast Travel) switch.
2. Stand still on flat ground near a town shop, a landmark center, or a confirmable boss, and wait for the auto registration; opening F8 also checks. Landmarks need two stable samples, usually 3–6 seconds. Other wild places: F8 → type a name → "register current position"; if left empty, an identified place name is preferred, otherwise an auto number. Avoid doors, walls, roofs, water, and mounts.
3. Later, standing still in a single-player world with the map, inventory, and other vanilla menus closed: F8 → "return to nearest town" or pick a station → "confirm teleport". No fee; potions, equipment, experience, and the respawn point are untouched.
4. After teleporting it holds the landing point for about 1.5 seconds so the original game can load terrain; it ends after detecting standing still twice in a row. On a clear fall, drift, or no stable footing within 8 seconds, it attempts to return to the starting point. That is a failure safeguard — the real loading and return effect still need in-game acceptance, and no landing point is guaranteed absolutely safe.
5. At most 32 stations per character-name + world-seed + world-name combination. A renamed character starts a new record; same-named characters in a same-name same-seed world share the record. Old saves cannot fabricate historical landing points — after installing, stand still near the shops once; having town labels does not mean having a teleportable landing point. The old mod's `.sav` files are not imported or moved for now.
6. "Remove selected station" deletes only that record and changes no place. Disabling and removal keep all station files and game saves. The nearest-town reference is stored separately in `last_city.bin` in the same directory.

Close with F8 or the window's top-right. Starting is forbidden while dead, airborne, moving, or in an action; it locks in multiplayer or when the network state cannot be verified. During the safeguard, do not switch characters or exit; as soon as the identity or network changes, everything stops immediately — the new character is never moved. Pet and mount following are unverified; this version is single-player only, after dismounting.

## Independent boundaries

| Item | Value |
| --- | --- |
| DLL | `mods/fast_travel.dll` |
| manifest ID | `local.cubeworld.fast_travel` |
| Manager ID / switch | `FastTravel` / `fast_travel.enabled` |
| Data | `data/fast_travel/world_<seed>_<worldhash>_player_<namehash>/stations.bin`, `encounters.bin` (separate observation history) |
| Permissions | Overlay, RawMem, Writes; read-only world names, places, shops, boss records, and the name word lists; only after the user confirms a teleport, writes the original character's coordinates and velocity through the SDK's protected interface; auto-registration writes only this mod's data and installs no game function hooks |

None of the other five mods is a dependency. The source reuses the map's naming rules and the explorer's independent font helper without depending on their DLLs. While the old launcher still requires the map DLL file, the map can be disabled — the original removal protection cannot be bypassed.

`Manage-Mod.ps1 -Mod FastTravel -Action Disable/Enable/Uninstall` follows the existing management flow. Removal moves only this DLL into backups, keeping data and saves; positions already legitimately saved are not auto-reverted by disabling. The installer strictly verifies the game, loader, and candidate DLL hashes; unknown versions or an unknown existing DLL refuse to overwrite.

`Build-FastTravel.ps1` in the root only builds and runs offline checks; it never starts the game. The latest build, install, and real acceptance are recorded separately — see the 0.1.3 validation record (local validation records are not published with the source); the original teleport boundaries are in the 0.1.0 record (local validation records are not published with the source), the sources in [SOURCES.md](SOURCES.md).

The landing-point auto-discovery checks at most once every 3 seconds while standing still, with at most 9 coarse nearby place records; the shop entity tree caps at 4096 nodes, and the scan is skipped near an existing town landing point. The independent boss observation runs at most once per second over at most a 7×7 fine grid — 4096 spawn records per cell, 8192 total, and at most 64 tree nodes per entity ID lookup; on truncation or ambiguous linking, nothing is confirmed arbitrarily. The original quest type-5 supplementary path still queries a 3×3 fine grid on demand every 10 seconds. It never generates distant map data or calls the game's random functions.
