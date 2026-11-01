# Batch Actions: basis and investment decision

Verification date: 2026-10-28. The target is the current hash-pinned 2013 Alpha playtest copy, reducing the repeated actions when buying empty bottles, filling water, and brewing potions. 0.1.1 switched to the visible quantity window per the user's follow-up feedback, with A / D changing 10 per press.

## Reuse decision

- The current vanilla already supports holding the craft button, crafting continuously at roughly one round per second. This was confirmed by the loop near the pinned local `0x48f1a0` and its craft calls; it still completes one unit at a time, with no unified buy-and-craft-up-to-ten operation. The user has chosen in this conversation to batch ten uniformly.
- The [Omega official patch index](https://wollay.com/patch-notes/) was latest at [2026-10-27 v0.1.14](https://wollay.com/patch-notes/2026-10-27-0-1-14-quest-rewards-and-gameplay-fixes/) at check time. Its crafting UI and experience fixes belong to a different version; there is no evidence they apply to the local Alpha loader directly; the current game is not upgraded or migrated.
- Third-party write-ups of an old "key helper" were found, including simulated multi-click and long-press methods; without a confirmed original-author license, or evidence of compatibility with the current Qube Loader and Inventory Sort, nothing was downloaded. The checked [Cube World Alpha Mods](https://github.com/Gapagapi1/Cube-World-Alpha-Mods) is a config/server mod under another old framework and offers nothing directly reusable for this batch feature. No ready-made alternative verified against the current combination was found — that does not mean none exists.
- The recommendation is to continue this small feature: a standalone DLL, pick the quantity then confirm, reusing the native charging and recipe logic, with no reward-system or economy-number expansion.

## Local native code and pinned-program verification

Referencing `external/CubeWorld-Reversal/cube/control/GameController.cpp` while checking the disassembly of the local `playtest/Cube.exe`. That decompilation project contains mis-names; the function's actual behavior is the basis below.

| Location | Actual behavior |
| --- | --- |
| `0x47b600` | `__thiscall(GameController*, int button)`, button 1 is right-click; the vanilla branch checks the player cell, equipment, shop selection, balance, and carry cap, then adds the item and charges |
| `0x47b1b0` | The current equipment hover query, used to avoid double-running the native equipment operation |
| `0x47b450` / `0x47b550` | Player-inventory / shop-selection queries; the shop Widget is at `GC+0x80095c`, the temporary inventory at `GC+0x800c0c` |
| `0x48f1c6`–`0x48f1dd` | Copies the recipe Widget's `+0x16c` to `GC+0x1000e7c`, starting the vanilla crafting |
| `0x4709c0` | Though named `onItemPickup`, it actually consumes the staging requirement list, checks the carry cap, and adds the craft result; it must not be called directly when materials are insufficient |
| `0x59cff0` | Ordinary health potions need heart-flowers `(11,22)` and water bottles `(11,26)`, one each; water bottles need empty bottles `(11,12, material 24)` |

The native `Item` is `0x118` bytes. An inventory cell is `count + Item`, a requirement cell is `Item + count`, both `0x11c`, in different orders. Target items are compared with native semantic fields, ignoring uninitialized padding; only unmodified ordinary supplies are allowed. Inventory reads are capped and protected; cross-page reads still go through the SDK segment by segment.

## Loader and existing mods

Referencing `external/Qube-Loader/modloader/src/game/gamehooks/hookbus.cpp`, `rawpool.cpp`, `src/game/pickup.cpp`, and `src/game/capture_detour.h`. The generic interceptors run synchronously before the native call, and re-entering the entry runs the shared subscribers in order; this mod uses a thread-local flag to stop itself from re-expanding a batch.

Inventory Sort already intercepts `0x47b600`, declared Thiscall with one integer argument. This mod declares the same and registers late at READY; buying empty bottles passes through the existing protections each time and does not bypass Inventory Sort. The craft entry is mutually exclusive with the loader's on-demand ITEM_PICKUP capture, which none of the current six modules use; future combinations must be verified separately.

Pinned program SHA-256: `84A7A132A84D4282338E7EA45A1940D32066D64E39CBAFED8F2418D5A6DC30BF`. Loader SHA-256: `D729D26FF4F91EED453160FBE4D24D9430043AE6A7165CF3A3AF8A92C24B7A31`. The installer verifies both separately and upgrades no shared framework.

## This round's live check and UI fix

`research/inspect_batch.py` made read-only queries against the actual playtest process: the character had 50 empty bottles, 7 heart-flowers, 15 level-37 health potions, and 50 level-33 health potions; the shop's empty-bottle native fields match this module's semantics. This confirmed the empty bottles were at the vanilla cap at the time — the concrete reason no more could be bought then. It cannot prove that every earlier failed purchase was due to this alone. The record is in `research/evidence/batch-actions-0.1.0-live-fields.json`.

The old version, when already at 50, entered no batch handling and had no separate quantity window. The new version lets this case into the display layer, explaining with "owned 50 / 50, max this round 0", while the execution layer still refuses the overflow.

The quantity window's input takeover references the loader's `src/overlay/overlay.cpp` and `src/hooks/input_block.cpp`. During takeover the game's own mouse hover is not a stable trade target, so the original shop Widget, cell address, and full item identity are captured and re-verified per unit after confirmation. Native `0x47b600` button 2 also enters the buy branch, skipping button 1's right-click menu handling; it temporarily changes only four UI/input fields — `bagWidget+0x184`, `shopWidget+0x184`, `GC+0x8008f0`, and `GC+0x11b` — restoring them per unit. Money and item data are untouched. The production-path checks verified the original hover loss, partial binding failures, and restoration; the real UI and game-call effects still need post-update acceptance.

## This font tweak

Keeps the original game font, with normal stroke weight, pixel alignment, and the private font sampling; no changes to the shared default font, item data, or interaction. Independently compiled and updated — see validation record (local validation records are not published with the source).
