# Shop Purpose Guide 0.3.2

With the map open, the top-right of the screen shows nearby discovered shops' names, distances, and two lines of purpose/common-goods notes; closing the map hides it. The original shop name labels on the map are kept, and no purpose text is added next to map places.

The notes for the inn, item shop, weapon shop, armor shop, identifier's shop, smithy, workshop, and clothier were compiled from 2013 Alpha references. Common goods do not represent current stock. The full reference table and the English names are in this package's `shop-guide-sources.md`.

Keep launching the development copy via `C:\Projects\CubeWorld-Mods\playtest\NavigationLauncher.exe`. Press M to open/close the map; right-clicking a sidebar entry or a map shop name still navigates. Shop recognition, waypoint coordinates, and the existing cache format follow 0.3.1.

The install script is the project root's `Install-TownLabels.ps1 -Target Experiment`; the target game must be exited normally first. The script backs up the old mod and navigation data, replaces only the navigation DLL, and verifies the original program, framework, config, and save hashes stay unchanged. The formal copy was not updated this time.
