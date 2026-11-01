# Fast Travel 0.1.3: fix for missed already-seen, already-fought bosses

Boss observation is independent of standing still, moving, and casting, querying at most once per second, and additionally covers static bosses without quest type 5. Once reliably linked, the name and class are recorded immediately; existing F8 places are just filled in, with the personal text and the original landing point kept. The observation info is saved separately in encounters.bin for the same character and world, surviving entity removal and restarts; with several candidates and no clear target, none is guessed.

Teleport landing points still require dismounting and standing still — observing a boss never turns an aerial position or a spawn position into a teleport anchor. Old bosses never read but already removed cannot be fabricated. CWTRAVEL1/2/3/4 places stay compatible; the original F8 and manager switch are unchanged.

After the game and launcher fully exit, run the root's Install-FastTravel.ps1 — it updates only the playtest copy by default and backs up the saves and this mod's data. For the actual install state and the offline/real acceptance boundaries see the validation record (local validation records are not published with the source).
