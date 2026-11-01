# Fast Travel 0.1.1

Adds editing of registered places' names and multi-line notes. F8 → select a place → "edit name/note" → "save changes"; you can record the boss class, difficulty, completion status, and your next plans. The list shows a note summary, and the full text scrolls after selecting. Editing changes no teleport landing point or town number.

Old `CWTRAVEL1/2` places keep loading; adding or saving uses `CWTRAVEL3` with notes. Before saving it preserves and verifies `stations_backup.bin`; a write failure attempts recovery. The installer backs up the full saves and this mod's data; rolling back to the old DLL requires restoring the matching old-format data.

After fully exiting the game and launcher, run the root's `Install-FastTravel.ps1`; it updates only this mod in the playtest copy by default, reusing the original manager entry and switch. No automatic game start; Chinese input, actual saving, and the relaunch effect are still pending in-game acceptance. See `../../fast_travel/README.md` and `../../research/evidence/fast-travel-0.1.1-validation.md`.
