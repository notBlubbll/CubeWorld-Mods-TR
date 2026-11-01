# Fast Travel 0.1.0 playtest package

After the game and launcher fully exit, run the root's `Install-FastTravel.ps1` to install the playtest copy. The original mod manager has an independent "旅行驿站" (Fast Travel) switch; the other mods and the original switches are kept.

It follows the Trader Fast Travel flow: standing still next to a town shop records the place automatically; F8 → return to the nearest town or pick a station → confirm the teleport. Wild places can be registered manually. The full 64-bit coordinates are written through the original framework's protected interface; the old launcher and old DLL were not installed. Records are kept per character and world, at most 32 points. Single-player only, after dismounting; no new buildings or NPCs, and no fee. Real far-distance loading, pet following, and all combinations are still pending in-game acceptance.

After building you must update `release.json`'s DLL hash — do not just replace the DLL. The full notes are in `../../fast_travel/README.md`; the validation boundaries are in `../../research/evidence/fast-travel-0.1.0-validation.md`.
