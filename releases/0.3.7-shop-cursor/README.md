# Navigation 0.3.7: right shop-list pointer

Fixes the cursor being covered by the panel when hovering the right-side "shop guide" list of shops, inns, and so on after opening the map with M. Now, inside that panel and with the game in the foreground, a white pixel hand is drawn last; its position uses the pick hit-test coordinates, and right-click navigation is unchanged. The pointer looks the same as the existing one in the left explorer panel.

This is an update to the existing `waypoint.dll`, keeping the "Navigation" entry and the `waypoint.enabled` switch, adding no new mod and forcing no dependency. It changes no shop names, purposes, waypoints, storage format, potion prices, or the system cursor state. The hand is not drawn with the map closed or outside the panel.

Update with the root `Install-TownLabels.ps1` — playtest copy only by default, requiring the game and launcher fully exited, backing up the full saves and original map data, and keeping the other mods, config, and switches. The formal directory additionally needs explicit authorization. The user had disabled Map Light; it stays off — no bundled upgrade of it or the shared framework.

2026-10-24 23:15 +08:00 — installed into the playtest copy. The offline logic, the real-DLL handoff, the install protections, and the 1280×800 / 1920×1200 hidden-drawing previews passed; the in-game cursor effect still awaits the user's confirmation. The full record is in the project's `research/evidence/town-0.3.7-validation.md`.
