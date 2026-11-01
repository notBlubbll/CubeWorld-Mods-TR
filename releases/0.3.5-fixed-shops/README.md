# Duplicate Inn Nameplate Fix 0.3.5

The old version treated the shopkeeper's current coordinates as the shop position. After an innkeeper walked around, nearby new coordinates could be read as another inn, piling up multiple Chinese nameplates.

This version reads the shop's original generated position. When the matching inn is approached and loaded again, nearby same-category old records are folded into the nearest fixed shop position, keeping the other real inns and the records of unloaded regions; the original world, destinations, and data format continue as before. The map down arrow and shop guide follow 0.3.4.

The Win32 Release build and the 44 town checks passed, covering this cache's three duplicate inn coordinates, different inns, partial region loads, repeated scans, and old-cache compatibility. 19 install-flow checks passed in both PowerShell 7 and Windows PowerShell 5.1; the 41 standalone mod-management and 82 manager-status offline checks passed. The in-game display and the real running of every mod combination are still pending acceptance.

Keep using the playtest copy's original "导航启动" (navigation launch) or the mod manager's "playtest copy" entry. Enter the new world where the problem appeared, approach that inn, and press M to verify: only one "旅馆" (inn) name remains at the same spot, and the marker after selecting is still the down arrow.

This is still the original "Navigation" module, file name `waypoint.dll`, adding no new manager entry; the original independent switch stays in effect. Future reinstalls go through the root `Install-TownLabels.ps1`, which requires the target game and launcher to be fully exited and does not auto-enable a map module that was turned off.
