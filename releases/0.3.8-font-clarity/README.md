# Map Waypoint Navigation 0.3.8 (development test copy)

This font tweak: keeps the original FangZheng CuYuan glyph shapes, improves pixel alignment and sampling, and lightens the black outline on the map/explorer panels; no layout or gameplay changes. The playtest copy was updated as an independent backup; the real look is still pending a re-launch to confirm. See font record (local validation records are not published with the source).

Goal: mark a destination on the vanilla world map and, after closing the map, keep showing the relative direction and horizontal distance, so you can keep moving even in combat and after turning around.

2026-10-24 23:15 +08:00 — 0.3.7 installed after backup: fixes the cursor being covered by the panel when hovering the shop/inn list of the right-side "shop guide". Only while inside the right panel and with the game in the foreground, a white pixel hand is drawn last, with the fingertip matching the right-click shop coordinates; no redraw when leaving the panel or closing the map, and no changes to the system cursor or click behavior. The left-side region explorer keeps using the installed standalone 0.1.1; the map light stays disabled. This pass only replaced the map DLL; no other mods, the loader, the launcher, or the formal copy were updated. For check and install evidence see `research/evidence/town-0.3.7-validation.md`; the in-game cursor effect still needs confirmation after a restart.

2026-10-24 16:46 +08:00 — 0.3.6 installed as an independent backup: supports the optional region explorer pick, reuses one navigation target and the down arrow, and keeps shop names clear of the explorer panel. Without the region explorer enabled, the original interaction is preserved; it is not made a map dependency. The region explorer is managed by its own DLL, install script, and manager switch; this pass did not update the potion shop, the shared framework, or the formal copy. The actual DLL was checked in the synthetic SDK for handoff, world scoping, storage failures, and standalone runs; in-game verification is still pending the user. See `releases/0.3.6-explorer-bridge` and `research/evidence/explorer-0.1.0-validation.md`.

Installed fix 0.3.5: shop names use the original generated position, so a shopkeeper's current position after walking around is not recorded as another shop. Three adjacent inn records in the new world cache were used for offline regression; the real old cache tidies itself automatically when the matching shop is approached and loaded again, without clearing other towns, worlds, or the original navigation destination. The 44 town checks and the update-protection checks passed. 2026-10-24 12:56 +08:00 — after the user confirmed the exit and authorized it, the playtest copy update was completed, with the full saves, old mods, and waypoints backed up; the game was not started, and the real inn display is pending acceptance. See `releases/0.3.5-fixed-shops` and `research/evidence/town-0.3.5-validation.md`.

Installed fix 0.3.4: after selecting a shop or placing a manual map waypoint, a gold pixel arrow sits above the target, pointing down at the exact target coordinates, with a thicker shaft and a modest overall scale-up; still no added "destination" label or duplicate shop-name plate. Names automatically avoid the arrow; the right-side guide, the top target, and the distance stay. 2026-10-23 22:14 +08:00 — after verifying the game was closed, the playtest copy was updated; the saves and old mods were backed up, the potion mod untouched. The Release build, the 32 town checks, and two standalone previews passed; the in-game effect still needs checking.

## Shop sign names

Only the entities the original game draws shop signs for are read (category 3, roles 128–135), adding names for the inn, item shop, weapon shop, armor shop, identifier's shop, smithy, workshop, and clothier. No furniture is scanned, no separate label is added for crafting benches, and no regions are force-loaded. The right-side list shows the nearest shop of each nearby category; right-clicking a map shop name or a list row sets that exact position as the navigation target; the rest of the map keeps free waypoints.

Map labels avoid overlaps; the facility cache and the navigation waypoints are stored separately, keeping the current world scoping and the original `CW_WAYPOINT_2` waypoint format. The facility cache is now `CW_TOWN_2`, which automatically ignores 0.3.0's broken furniture cache and rediscovers the real signs. The cache holds at most 512 entries, the entity walk touches at most 4096 nodes, and it refreshes at most once per 1.5 s.

0.3.5 keeps `CW_TOWN_2` and, each time a trusted fixed shop position is discovered, folds old records of the same category within 8 horizontal units and a vertical difference under 4 into the nearest fixed position. Old records are kept when the matching shop is not loaded; distant inns, different categories, or different floors are not merged. This range targets this incident's duplicate records about 2–3 steps apart — it is not a whole-map merge of same-named shops.

The latest files and notes are in `releases/0.3.6-explorer-bridge`; the install script `Install-TownLabels.ps1` updates only the playtest copy by default, requires the target game and launcher to be fully exited, and verifies and backs up the full saves, old navigation, and map data first; the other mods and the original switches are kept. In 0.3.1's real map, 5 shop names were verified adjacent to their original signs, with the log showing `loaded=5 new=5 known=5 scan=0 ms`. That timing only means under 1 ms at millisecond sampling — not a performance stress test. Untested categories and other scenarios are in `research/evidence/town-0.3.1-validation.md`.

0.3.2 expands the right-side list into a "shop guide", giving two lines of purpose or common goods under each shop category. The content was checked against 2013 Alpha references; the source table is at `research/evidence/shop-guide-sources.md`, the copy in `town_guide.hpp`. The purpose text is not added next to map locations; shop names and right-click navigation stay. The guide only runs while the native M map is open and drawing is ready, and hides when the map closes. The goods notes do not represent reads of live stock.

The new sidebar's Release build passed, plus same-code previews at 1280×800, 800×560, 800×480, and with an empty list, including text-width checks for all 8 category blurbs and list click-hit checks. The PC automation tool reported `GetCursorPos failed: 拒绝访问 (0x80070005)` (access denied) twice in a row when launching the game; retries were stopped, no system permissions or settings were changed; the user was asked to open from the original entry and check show/hide. No claim is made that the new sidebar was tested live in-game at this point.

## File isolation

0.2.0 unified the style of the navigation-related UI: fonts from the game's local `resource1.dat`, a black-and-white stepped border, a deep-blue panel, gold block wayposts, and a separate distance plate. The map hint, the waypoint name plate, and transient notices share one drawing function; the no-target hint and secondary text were shrunk. The font file is only read from the local game and is not redistributed with the mod; on absence it falls back to a system Chinese font. Navigation actions, the distance algorithm, and the `CW_WAYPOINT_2` format stay compatible; existing waypoints are not cleared.

## Usage

1. Launch from the playtest copy's `NavigationLauncher.exe`; you can pick the freshly imported original character and world; for the formal game, use the "导航启动" (navigation launch) entry in the formal directory.
2. Press `M` to open the vanilla world map; right-click in the map to place or replace the yellow waypoint.
3. Close the map: the top direction bar shows the target bearing and horizontal distance; left/right arrows appear when the target is out of view.
4. Within 20 world units it shows "approaching destination", and the waypoint is kept.
5. With the map open, press `Delete` to cancel the waypoint. After quitting and re-entering the same world, it reads its own waypoint file.

The `m` in the UI currently counts game coordinate units and awaits measured calibration. Map picking intersects the full original map transform matrix with the horizontal plane; 3D terrain relief can still cause landing-point offsets.

0.1.1 fixed the missing projection matrix: 0.1.0 had mis-computed distant towns as points near the player, so the number went up while walking toward a town. Old waypoints are no longer read — right-click once to re-mark; character and world saves are unaffected.

## Local build

Run `Build-Navigation.ps1` from the root, then `Prepare-Playtest.ps1`. The preparation script does not start the game and does not copy old saves; if `playtest` already exists, it refuses to overwrite in order to protect the playtest saves.

`Install-Navigation.ps1` is for the machine-local formal directory the user specifies: it requires the game/server to have exited, verifies the tested program hashes, backs up and verifies the saves and configs first, then adds files. It stops on same-named files to avoid overwriting; it never starts the game. The formal entry can load navigation; the original entry still works for playing without navigation.

The formal entry's error 740 is fixed: when Windows returns `ERROR_ELEVATION_REQUIRED`, the launcher re-runs itself once through normal UAC consent and then creates the game child process it owns; the user must personally confirm the Windows prompt. Cancelling or failing again stops it — no permission loops. No game compatibility settings or system security settings are changed. See `research/evidence/launcher-740-repair.json` for the repair record.

Dependencies pinned to:

- Qube-Loader `4c3dad5a4665ffcc40117aef7fe6da6fdea61ca9`.
- ImGui `8936b58fe26e8c3da834b8f60b06511d537b4c63`.
- MinHook `c3fcafdc10146beb5919319d0683e44e3c30d537`.
- Reverse-engineering reference CubeWorld-Reversal `e6e906e4b2644806ef19c2e1f12ebc636c347f7c`.

The local Qube changes include loading the game's original Chinese font before the first draw (the system Microsoft YaHei is fallback only) and fixing the zero-client-area problem of the hidden draw-probe window on high-DPI screens. The patches are kept in `research/evidence/loader-chinese-font.patch` and `loader-high-dpi-probe.patch`. The local tool paths are written in the build scripts. Build with Release to avoid the local Debug linker's MSPDB140 version problem.

## Basis for the map integration

Located via static disassembly of the original program, cross-checked against the reference sources:

- `GameController+0x8006e4`: the `M` world-map toggle. The SDK's plain Widget field currently named `mapOpen` is not used by this feature.
- `GameController+0x800d44`: the embedded WorldMap; `WorldMap+0x4` holds the projection matrix, `WorldMap+0x44` the view matrix. Only the column-major `projection × view` combination is used for map picking — the view matrix alone must not be treated as the full projection.
- `GameController+0x1000e4c`: the map drag offset. The original program converts it to fixed point at `0x4adf13`, adds it to the player coordinates, and uses it as the map draw origin.
- Player coordinates use the three 64-bit fixed-point values at Creature `+0x10/+0x18/+0x20`, avoiding needless floating-point precision loss at far coordinates.
- World waypoint scoping is determined by the world name and seed together; same-seed worlds with different names are stored separately.

The navigation mod declares the `Overlay | RawMem` capabilities and requests no game-property writes and no custom game-function hooks. Qube itself still uses its original graphics/input hooks to provide the overlay. Qube's version check stays on at load; mismatched versions are not forced to run.

## Validation status

0.2.0: the framework and mod Release builds passed, 51 checks passed. In the playtest copy's original level-4 character scene, the navigation bar and distance plate were seen, and the logs confirmed the game's original font loading, the 0.2.0 mod, and the original world waypoints being restored; the user checked the style and confirmed the direction was close to expectations. After backing up, the same build was synced to the formal copy; the formal copy has not been restarted since this style update, and that is not written up as tested. During the install, the game program, configs, and all save hashes stayed unchanged.

2026-10-22: Release builds done, 51 checks passed, covering the real log-matrix regression, the distance decreasing while moving toward a fixed target, increasing when moving away, and old waypoints being invalidated. Live playtest checks confirmed the framework and 0.1.1 mod loading, Chinese display, and right-click waypoints on the native map; the user reported "walking and the number is getting smaller". Detailed evidence is in `research/evidence/navigation-0.1.1-validation.md`.

The following full scenarios are not yet fully accepted:

- Zoom, drag, and reopen the map and check the landing point; check offsets from terrain height.
- Direction and distance changing correctly when turning, strafing, running away, and coming back.
- Restore after restarting the same world; new worlds and same-seed different-name worlds not inheriting waypoints; cancellation persisting after restart.
- The playtest character staying normal through combat, death, and world transitions; the original directory's program and configs untouched.

Compile checks and math checks do not replace the in-game acceptance above.
