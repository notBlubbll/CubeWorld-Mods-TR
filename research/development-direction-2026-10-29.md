# Current development direction: making equipment grinding better-grounded

> Update date: 2026-10-29, Asia/Shanghai. Scope: the development direction and the next feature's boundaries; not a completion record of a new mod.

## Latest choice: original loot plus an extra draw

2026-10-29, in this conversation the user explicitly chose "the boss's original drop still comes as usual, plus one extra equipment draw", and authorized development per the discussed plan. **The current priority shifts to the standalone Boss Draw small version**: 40% your class's exclusive, 40% this boss's actually-worn candidates, 20% other classes or universal jewelry; after three consecutive misses of your class's exclusive, the fourth only guarantees the class; the level and quality are based on the boss at kill time, with a manual draw. With no worn candidates, the shares split 2:1 into the other two pools. For the rules and implementation boundaries see [Boss Draw](../boss_draw/README.md).

The vanilla death loot and quest rewards stay. This item converts no equipment, changes no vanilla probabilities, and develops no paid currency, character cards, or big long-term gameplay. Boss intel remains useful as a later reference, but "intel first" is no longer a reason to block this authorized development. The original plan below is kept as the prior decision and research background; statements there like "the class pity has not been chosen" have been superseded by this latest choice. The build and actual install state are defined by this validation record — in-game acceptance is not inferred from design docs.

## Current goal and the choices already made

The user has not gotten Omega beta access and continues from actual play in the existing 2013 Alpha with the current character. Omega serves as a feature and trade-off reference; gaining access or switching versions is not a development prerequisite.

This round the user explicitly prioritized "equipment grinding" — the pain point being that after beating many bosses, gear suited to their class rarely drops. Earlier, in "Find out the class-equipment drop probabilities", the user chose to first figure out which bosses are worth farming, keeping the vanilla drop mechanics; the deep loot verification of the local program has since been completed. That existing choice cannot be ignored by restarting from "should the drop rates change?".

Existing conversation anchor: `01a17882-5e69-7bf0-9638-d8d8bbaa0861`, title "Find out the class-equipment drop probabilities"; supplementary context "Map-marker development area", `01a163c6-fa2d-7b01-9c28-f640504284bb`. The conversations confirm the user's choices; technical facts follow the verified reports and the current implementation.

Core basis: [2013 Alpha equipment-drop reverse-engineering report](loot-mechanics-2013-2026-10-27.md). It separates the enemies' own equipment, extra random equipment, valid quest rewards, and identified items — it must not be lumped together again as "one quarter per class". The report's offline comparison of original instructions does not mean every real combat settlement has been accepted.

## What to prioritize next

The next feature prioritizes researching and expanding **the boss's actual equipment intel in Region Explorer**. Region Explorer 0.1.5 can already show the boss class's confirmed, limited prediction, or unknown, but not yet a full display of the actual equipment candidates. A matching class alone does not prove worth farming; the next step is to ground the choice in the actual items.

Minimum scope:

1. Reuse the existing verified place–boss identity links and read generated, reliably confirmable entities; generate no far-away bosses and replace no game random numbers.
2. List the non-empty wearable-slot candidates actually used by the death-loot path, showing the item kind, quality color, level or Power, and the vanilla class fit. The UI's class judgment should use the current character and the vanilla restrictions — the user should not re-enter a class.
3. Check whether the entity satisfies the own-equipment drop branch's conditions. Describe that branch's draw rules only with sufficient evidence; the candidates are not a guaranteed next-drop list, let alone a fixed signature-artifact table for a specific named boss.
4. Clearly separate own-equipment candidates, extra random drops, and quest-completion rewards. Valid quest rewards may go straight to the bag; no gear appearing on the ground must not be recorded as no reward at all.
5. Show unknown when far and unloaded, reads fail, or multiple candidate identities are ambiguous; a predictable class does not mean confirmed equipment. The first version adds no whole-world equipment prediction.

First solve "does this target have the gear I want, and is it worth fighting?". The first version need not also build a complex wish list, auto loadouts, route optimization, or precise total drop-rate prediction; whether those are needed is decided by the real grinding feedback.

Reuse the current Region Explorer's entry, place list, fonts, and manager entry as an increment of the existing feature. Navigation keeps handling directions, Fast Travel keeps handling visited places and personal notes — these three are not merged into one big mod that must be enabled together. If the equipment-intel display and cross-module persistent records need new messaging, evaluate that separately; existing data formats are not casually changed just to write a plan.

## How to weigh the current six active features

The project keeps the source and manager entries of seven standalone features; per the latest install record, the playtest copy has Map Light removed and the other six kept. The following is the doc-vs-install-record reconciliation, not a re-run of the game this round.

| Feature | Current record | Next investment |
| --- | --- | --- |
| Region Explorer 0.1.5 | Class confirmation, limited prediction, difficulty and place info | The main incremental entry for grinding intel; keeps the original switch. |
| Fast Travel 0.1.3 | F8 teleport, places and boss observation, multi-line personal notes | Keep the live info reliable and the teleport/saving usable; use the existing notes first to record targets worth farming. Observed is not killed. |
| Navigation 0.3.7 | Direction and distance, shop guide, optional explorer target handoff | Keep it working; fix the cursor and picking if needed; no more decoration or map-interaction rewrites for now. |
| Potion Shop 0.2.0 | Sells one tier below the best craftable, priced with progression | Supports continuous grinding, keeps the existing rules; prices and progression are not adjusted this round. |
| Batch Actions 0.1.1 | Quantity windows for empty bottles, water, and potions | Prioritize the acceptance of real trades, material consumption, and save/restart; passing UI or DLL checks is not a passing real trade. |
| Inventory Sort 0.1.6 | Chinese sections, detail yielding, take/place and sale slot preservation | The confirmed operations are kept, necessary fixes continue; no parallel trading or full loadout system alongside. |
| Map Light | Removed 2026-10-28 at the user's request | Kept on record; reinstalling is up to the user. The old table's "0.1.0 installed, just upgrade" no longer applies. |

Fast Travel, Region Explorer, and Batch Actions still have the real-game acceptance items listed in their docs. Fix blocking issues per actual feedback; do not indefinitely postpone the grinding intel behind "finish testing every mod first". New intel features must also be verified proportionally for reading, identity, display, and combination with the existing modules.

## Directions not to expand yet

- Universal smart class drops, failure-count pity, item class conversion, quality/level boosts: none of these is the currently chosen plan. Only if clear problems remain after the intel improves, and the user re-chooses gameplay changes, are they discussed concretely.
- A separate adventure journal: the existing stations already have places, boss observations, and personal notes; use the existing capability first — no new storage and UI for the same recording need.
- Big skill trees, world generation, full camp/villager economy: not the current pain point; no investment for now.
- Auto-hiding all off-class bosses: universal jewelry, extra random equipment, and quest rewards may still be valuable; provide the basis and let the user choose — off-class places are not silently hidden.

The convenience features have already cut resupply and repeated map runs; now verify whether that time converts into better-grounded equipment pursuits. Killing more bosses faster can still lack effective upgrades; whether target info is useful enough is measured by whether the user can pick out targets worth challenging and worth revisiting.

## How Omega's latest changes affect the judgment

2026-10-29, querying the official patch index, the latest is October 28's **v0.2.0**. It added the Journal: a starter guide plus knowledge gradually accumulated through exploring, talking, and reading; the discovery records are shared across all characters of the same account. The author also says the backstory and journal will keep growing. This updates the October 25 suggestion that "the journal might be an uncovered new direction", but it does not prove the late-game goal or equipment-acquisition problems are solved.

Sources: [official v0.2.0 patch](https://wollay.com/patch-notes/2026-10-28-0-2-0-journal-and-world-lore/), [journal notes](https://wollay.com/blog/a-journal-for-your-adventures/). Both read on 2026-10-29. No claim is made that Omega has equivalent boss actual-equipment intel, and no unplayable version is treated as a local ready-made substitute.

The [October 25 comparison and PDF](omega-mod-planning-2026-10-25.md) remain a historical snapshot and must no longer set current priorities by five mods or the old Omega state. Before adding features, the official and ready-made options are still refreshed per the project conventions; compatibility, licenses, and actual applicability of the same game build are verified before reuse.

## Accepting whether this direction works

Pick a few representative targets and check the intel before setting out and before the kill: can you tell whether a candidate suits you, distinguish "no relevant candidates" from "candidates existed but weren't drawn", and find targets you are willing to revisit? The existing quest rewards and bag changes are also checked.

Not drawing the one item you wanted does not automatically make the intel a failure; intel contradicting the real candidates, wrong identity links, or stating the unknown as certain must be fixed. This stage first verifies whether the information improves choices; no number of kills is promised to yield an upgrade.

This round only updates the development direction and reference entries. No drop code, equipment data, existing mod DLLs, or saves were modified; nothing was rebuilt, installed, or run. The "minimum scope" in this file is the next feature's boundary and cannot yet be called complete.
