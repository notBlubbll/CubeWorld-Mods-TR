# Map Down Arrow 0.3.4

When a map target is selected, a gold pixel arrow sits above the target with its tip pointing down onto the target position. The shaft thickens from 3 to 5 cells, and the overall size scales up modestly with the existing UI ratio. Still no target name is added and no second nameplate.

The shop's single name automatically yields to the arrow's area, so switching to the down arrow does not cover the text; the sidebar and top navigation stay as they were. Only the map visuals and nameplate yielding are adjusted — no changes to navigation coordinates, distance, storage, or the potion feature.

The candidate passed the Win32 Release build, the 32 town checks, and standalone previews at 1280×800 and 800×480. The previews use the actual drawing code with illustrative data — not real game screenshots.

2026-10-23 22:14 +08:00, after verifying the playtest game process had ended, the full saves and old navigation were backed up and this version installed; the potion mod stayed as-is and the formal copy was not updated. The entry is still the playtest copy's original "导航启动" (navigation launch); it takes effect on the next start, and the in-game effect awaits the user's confirmation. The install record is in `research/evidence/town-0.3.4-experiment-install.json`.
