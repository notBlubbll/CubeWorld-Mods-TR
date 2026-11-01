# Boss Draw: basis and implementation boundaries

Verification date: 2026-10-30. The user has clearly chosen "original loot as usual, plus one extra draw" and authorized development with the 40/40/20 card pool, the class pity, and manual draws. It is a new gameplay choice replacing the earlier "research equipment intel only" priority, and it does not change the vanilla death-loot algorithm.

## Reuse decision

This session queried the [Omega official patch index](https://wollay.com/patch-notes/), the [Alpha mod notes](https://gist.github.com/Oifan/a83b6956f8f6774c1400a6d9001ef70d), and related GitHub results, and found no ready-made equivalent verified to run on the local Alpha build; that is not a claim that no community project has the feature. An official feature for a different version cannot be dropped into the current 32-bit client. This round's direct fetches of some new patch pages failed, and no conclusion was drawn about the latest version's content or compatibility.

The recommendation was to invest in one small standalone version, verifying whether the extra draw improves the equipment-grinding outlook, without expanding into paid currency, event pools, character cards, enhancement systems, or a long-term economy. The existing local [loot reverse-engineering report](../../research/loot-mechanics-2013-2026-10-27.md), the Qube SDK, the independent font helper, and the manager and install protections are all reusable.

## Vanilla behavior verification

| Address / data | Use here |
| --- | --- |
| `0x59B180` | The vanilla death-loot flow; thiscall, two arguments, `this` is the embedded World (GC+0x2E4), argument 0 the dying entity. Called by the kill settlement at `0x59622E`; the local full call chain was verified. Only the original call is subscribed — never canceled or re-parameterized. |
| `Creature+0x60/+0x7E` | Category 1 plus the `0x200` boss bit; `0x800` excluded. Current HP must already be 0. |
| `Creature+0x13A4` | The vanilla contribution tree, keys at node `+0x10`, positive damage at `+0x18`. The local player's full 64-bit ID must exist. A player ID can be 0 and must not be mistaken for a null pointer. |
| Seven candidate slots | Only the off-hand, main-hand, chestplate, pauldrons, gloves, boots, and amulet used by the vanilla own-equipment branch; no extra ring slot. The full `0x118`-byte Item is saved. |
| `0x46EBE0` | The native inventory add-item method; thiscall, `this=player+0x11DC`, arguments an Item pointer and `-1` for the auto category. The vanilla quest reward calls it the same way at `0x596AD3`. This method does not modify the vanilla loot algorithm. |
| `GC+0x800A0C` | The current character save number. The recovered source's save call uses it as the character index for `saveEntityBlob`; the character name and class are also included in the storage scope. |
| `0x5F8530` / `0x4C7E80` | The verified quality formula and the level-curve float shape. The new card pool uses this module's independent random stream — it does not run the vanilla generator and does not consume global CRT state. |

Actual program SHA-256: `84A7A132A84D4282338E7EA45A1940D32066D64E39CBAFED8F2418D5A6DC30BF`. Existing shared loader SHA-256: `D729D26FF4F91EED453160FBE4D24D9430043AE6A7165CF3A3AF8A92C24B7A31`. The installer checks the whole-file hashes, and the runtime additionally verifies the PE and the key function bytes; version protection is never switched off.

## Independence and exception handling

- Uses the existing generic Raw Hook interface and hooks only the vanilla death entry. The current Navigation, Potion Shop, Explorer, Inventory Sort, Fast Travel, and Batch Actions do not touch that entry. An entry conflict found during initial detection or at READY refuses to enable; the loader is not upgraded.
- The death observer only takes a read-only snapshot on the original call's thread and submits it to a lock-protected bounded queue with world, character, and class scope; the FRAME thread verifies the scope, dedups, and persists — the draw and the grant still run on FRAME. Processing happens only in single-player with verifiable character, world, and save number. Idle rechecks of the context and the few identities of processed deaths run every half second; kills and explicit draw/claim recheck immediately, avoiding the per-frame session.get that walks the entity tree. A "disappearance" is never treated as a kill.
- The same death is deduplicated; a new valid kill is allowed after revival, entity respawn, or removal from the current entity tree is confirmed. Re-logging does not fabricate rewards from historical places, boss names, or target distance.
- The ledger has a format number, bounds, and a checksum, is backed up before writing, and is read back to verify; a corrupted ledger is not auto-rolled back to the old backup, to avoid re-granting already-claimed rewards.
- Before the native add-item it saves Claiming, then verifies the item's semantic fields show exactly one added. Failures/uncertainty never retry. The independent files and the vanilla character save cannot be committed atomically together; an exceptional claim locks that card and keeps the evidence for verification.
- Closing the UI, disabling, or removing deletes no draw records; different worlds and character scopes are kept apart. The added items themselves belong to the normal character save and are not recalled on removal.

The runtime interface checks use the actually built DLL; the business and UI callbacks use the production source with simulated memory/native calls. The first delivery did not start the game; after the 0.1.1 fix the user relaunched and confirmed the F9 window opens, while real boss recognition, inventory granting, normal save/restart, and performance while running together are still unaccepted.

## 0.1.1: locating the dead F9

This session's logs show 0.1.0 printing unsupported Alpha build at init, before the menu or the kill observer registered; it was not an F9 keybinding or player error. Read-only verification against the exact playtest child process: actual image base `0x2C0000`, preferred base `0x400000`, death-entry actual bytes `55 8B EC 6A FF 68 7B 20 5B 00`. Apart from the SEH handler address — which was correctly relocated — the instructions and version all matched; the inventory add-item entry and the death call site also matched.

0.1.0 treated the absolute address in `push 0x6F207B` as fixed bytes, so a normal relocation was rejected too. 0.1.1 keeps all 10 byte checks but compares that operand against `rebase(0x6F207B)`. Unknown versions, jump overwrites, and wrong handler addresses are still rejected; the game program, loader, and version protection are unchanged. Real-DLL regressions for low-base and high-base success and wrong-operand rejection were added. See live evidence (local validation records are not published with the source) and fix validation (local validation records are not published with the source).

## 0.1.2: one authorized missed-record compensation

The offline tool reuses `loot::decode/credit/encode` and only appends a sealed card — no draw and no native add-item are executed. The submission script binds this world, character, and observation record, verifies the exact game/DLL/tool hashes, and passes the real process-exit check; it backs up the old data, writes a one-shot processing credential first, then atomically replaces the ledger and reads back and checks the other files. An interrupted credential is not auto-retried; a completed credential refuses to append again. The log contains the old vanilla-encoded names, the identity scope is read only from the ASCII scope token, and unrelated logs are not required to be UTF-8. The data format is still CWBOSS01 and the normal card-pool algorithm is unchanged. See compensation validation (local validation records are not published with the source).

## 0.1.3: card flip inside the window

Reusing the existing `revealedAt` clock, it interpolates the width over 1.1 s with a fixed count of ImGui DrawList primitives, a few light dots, and a sweep; no game-state writes, no global random numbers, no background animation tasks. The claim button uses the same duration; result persistence and the native grant path are unchanged. The new checks cover only the geometry validity of each animation stage and the reward data staying unchanged; the real frame rate is not yet measured — see validation record (local validation records are not published with the source).

## 0.1.4: the continuously rotating quality card

Only the existing display function was extended: the card face is colored by its real quality, a fixed count of primitives does the eight-second loop in perspective projection, and the info stays fixed around it. It still reuses revealedAt with no new background tasks; the claim delay stays at the original 1.1 s, and the continuous spin does not block claiming. The geometry-and-data-unchanged checks at nine key moments passed. The equipment-model direction was not implemented — see record (local validation records are not published with the source).

## 0.1.5: unified UI and font clarity

Per the local frontend-design skill and the user's explicit regular-weight requirement, the local msyh.ttc face 0 (Microsoft YaHei Regular) is the draw's dedicated font — 20px, PixelSnapH, default stroke weight. The new font.hpp keeps the context-change, atlas-lock, and RendererHasTextures protections; the global font and the other mods are untouched. The UI reads only recordsValid, distinguishing corrupted records from a normal empty state, keeping the original actions and disable conditions. The previewer renders multiple sizes from per-command dynamic textures to avoid software-preview artifacts; see validation record (local validation records are not published with the source).

## 0.1.6: separating death observation from the render thread

2026-10-29 the user confirmed F9 opens but a kill adds no draw. Live 0.1.5 was loaded with a valid scope, nothing credited or granted, the ledger checksum valid, pending=0, nextSerial=2 — proving this kill was neither recorded nor granted. Vanilla valid quest rewards are generated by player class and go straight to the bag — see [the reverse-engineering report, section 5](../../research/loot-mechanics-2013-2026-10-27.md); class-matching gold gear alone cannot be attributed to the mod, and the empty record cannot reconstruct the specific boss.

The SDK's FRAME comes from D3D9 render (lifecycle.cpp, gameevents.cpp), and Raw Hooks dispatch synchronously on the original function's thread (rawpool.cpp). The old version bound the thread at the first FRAME, and death rejected other threads before reading the snapshot; deaths on different threads were silently missed, and the live thread id was not recorded by the old version, so it cannot be proven this was the only cause of that miss. 0.1.6 removes that precondition: the queue carries scope and entity identity, and all scope, ledger, and processed-death tables are managed by FRAME only. The SDK framecache's enabled flag is thread_local; the death thread reads the context live and does not read the render cache. A real second-thread callback check and a boss-snapshot rejection-reason log were added. See this validation (local validation records are not published with the source).

## 0.1.7: verifying the embedded World per the original call chain

2026-10-30's new live case: 0.1.6's death and FRAME were both thread 30172; the boss candidate was rejected by the context/receiver check and never reached the contribution/equipment snapshot stage. The earlier thread restriction was not the cause this time. The original program takes the World from GC+0x2E4 and passes the same ECX to `0x59B180` via `0x60C510` and `0x595A60`; the SDK's CubeSession.address is the GC. The old code wrongly required World==GC, and the test fixture made the same wrong call with GC, so the offline checks never caught the real-call deviation. It now strictly requires self==GC+0x2E4, without loosening the single-player, contribution, or version checks; context failure and receiver failure print separately.

The full evidence is bound to the local program's hash with 41 original instruction bytes matching — see the receiver-object basis (local validation records are not published with the source). The tests use independent literal offsets along that call chain: the old source fails, the new source passes, while the wrong-controller, spawned-world, dedup, and cross-thread checks are kept. See 0.1.7 validation (local validation records are not published with the source).
