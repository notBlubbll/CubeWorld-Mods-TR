# Boss Draw 0.1.4

Every time you help defeat a vanilla boss, all the original loot is kept and you get one extra equipment draw. Stand still in a single-player world, close the native menus, put down what you are holding, then press **F9**, click "flip the equipment card", view the result, then click "claim this equipment". After claiming, check the full stats in your inventory and compare with your current gear. Opening the window does not spend a draw by itself.

0.1.4 colors the revealed card face by its real quality and rotates it continuously for about eight seconds, with the name, level, Power, class, quality, and source fixed around it. After 1.1 seconds you can still claim immediately — no need to wait out the spin; it draws only inside the draw window and shows no equipment model. The playtest copy was updated as a backup; existing data kept, real in-game smoothness pending acceptance. See preview and validation (local validation records are not published with the source).

0.1.3 adds a ~1.1 s card-back gather, flip, and real-quality light dots; it draws only in the draw window and does not pause the game. The equipment is claimed after the animation ends; closing the window also keeps the saved result. The card pool, class pity, and data format are unchanged. The playtest copy was installed as a backup; in-game smoothness and the actual claim are pending acceptance — see the animation and validation record (local validation records are not published with the source).

## Reward rules

- Every hostile boss that enters the vanilla death-loot flow with this character's positive damage recorded counts once. A boss is defined by the game's `0x200` flag — ordinary monsters, pets, nearby kills you did not participate in, and simple disappearance or unloading do not count. Kills from before the first enable are not backfilled.
- Base split: 40% your class's exclusive weapon/armor, 40% non-empty equipment from the boss's seven vanilla wearable slots, 20% other classes' equipment or universal jewelry. The remaining branch divides into four equal parts inside itself: universal jewelry and one part for each of the other three classes. Real boss equipment can also fit you, so the "final share for your class" is not fixed at 40%.
- A boss with no worn equipment still gets one draw; its unavailable shares go 2:1 to your class and the other branch, ending at roughly 66.7% / 33.3%. The UI shows the base split for the current case.
- When the extra draw misses your class's **exclusive** equipment three times in a row, the next one guarantees an exclusive weapon/armor for your class. Drawing your class's exclusive equipment in any branch resets the counter; universal jewelry does not. The pity does not guarantee color, slot, or a stat upgrade, and it does not convert vanilla drops.
- The boss's worn items are copied whole, keeping the original quality, level, seed, and modifications. The other branches generate around the boss's level at kill time: the base level is `max(1, L−1 or L)`, floating ±0.05 along the vanilla level curve. Quality is drawn by the boss's tier using the shape of the verified quality formula but an independent random stream; a higher level does not force purple or gold.
- The new equipment uses only vanilla-legal types and materials; the ranger pool includes bows, crossbows, and boomerangs, with the four armor slots equally weighted. These are the new card-pool rules and do not copy the vanilla generator's weighting that misses crossbows and duplicates some pauldrons.
- The staging cap is 128 draws; claim in time when close to the cap. When full, vanilla loot is still kept, but no extra chances are added, and the HUD prompts you to claim first.

## Saving and boundaries

Draws are saved separately by world seed, world name, character save number, character name, and class. The boss level, candidates, and random seed are recorded after the kill; future upgrades do not improve old cards' rewards. The draw results and pity counter are saved first — re-entering cannot redraw. Disabling and removing keep the unclaimed draws and records by default; claimed equipment lives in the vanilla save, and disabling does not revert it.

Before granting, it saves an "issuing" record, then calls the native inventory add-item function and verifies exactly one item was added. On a call error or an unconfirmable result it locks that card and never auto-reissues. If an exceptional exit happens during a claim, keep `data/boss_draw/` and the logs for verification; the current first version blocks later claims of that card. It is not a cross-file transaction between the game save and the mod's files, so equipment and draw records are not guaranteed to hit the disk together under power loss and the like; normal save-and-exit remains the game's job.

Only the verified 2013 Alpha client and the existing Qube Loader are supported. Multiplayer, an unconfirmable character/world, unknown versions, or externally overwritten function entries refuse to grant. The vanilla death function is only observed — never canceled, never re-parameterized, never replayed; granting calls the vanilla inventory method and does not change drop rates, money, experience, potions, or the shared loader. The independent random stream does not call the game's rand/srand.

## Install and management

After fully exiting the playtest game, launcher, and manager, run `Install-BossDraw.ps1` from the root. The installer fully backs up saves, this module's existing files and configs, verifies the program and loader hashes, and keeps the other modules. It installs only the playtest copy by default and never starts the game; the formal directory needs separate explicit authorization.

The original "魔方世界模组管理器" (Cube World mod manager) provides the **首领抽奖** (Boss Draw) entry:

| Item | Value |
| --- | --- |
| DLL | `mods/boss_draw.dll` |
| manifest ID | `local.cubeworld.boss-draw` |
| Manager ID / switch | `BossDraw` / `boss_draw.enabled` |
| Data | `data/boss_draw/<world-character scope>/ledger.bin`, `ledger_backup.bin` |
| Shared dependencies | the verified Qube Loader and shared launcher; no dependency on any other feature DLL |

`Manage-Mod.ps1 -Mod BossDraw -Action Disable/Enable/Uninstall` follows the single-module flow. Removal moves only this DLL into the project backup, keeping the draw data and saves; it deletes no other module or the shared framework. `Prepare-Playtest.ps1 -Mods BossDraw` prepares a new playtest copy with only this feature, without overwriting the existing `playtest`.

Build and necessary checks: `Build-BossDraw.ps1`. Implementation basis in [SOURCES.md](SOURCES.md). Offline simulation and the real-DLL checks do not mean in-game kills, granting, save/restart, and mod combinations are accepted; for the real acceptance status see this delivery's record.

2026-10-29: 0.1.2 updated the playtest copy as a backup, reusing the manager's original eighth switch; the compensation card was saved, verified as 1 pending draw, unflipped, pity counter 0. The 36 protected files (including 8 vanilla saves) are unchanged, and repeated runs added no draws. Neither was the game started nor equipment granted directly; 0.1.1's F9 was confirmed by the user, while 0.1.2's new kills, actual flip-and-claim, and save/restart are still pending acceptance. See this compensation record (local validation records are not published with the source).
