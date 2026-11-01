# Map Target Pointer 0.3.3

After selecting a shop or a manual waypoint on the map, the target position shows only an arrow — no extra duplicate nameplate. The shop's original name, the right-side purpose guide, and the top navigation stay as they were, avoiding two identical nameplates on one spot.

Press M to open or close the map; right-click a sidebar entry or a map shop name to navigate. Navigation coordinates, discovered shop records, and the save format follow the existing version.

The install command is the project root's `Install-TownLabels.ps1 -Target Experiment`. The installer blocks overwriting a running target game, backs up the old mod and navigation data first, replaces only the navigation DLL, and verifies the original program, framework, config, and saves stay unchanged. Installing never starts the game.

The installed navigation DLL was verified to match this package; the full saves, old DLL, and waypoints were backed up, and the potion mod was untouched. The in-game display after relaunch still awaits the user's confirmation; the formal copy was not updated.
