# 2013 Alpha Equipment Drops: Original-Program Reverse-Engineering Verification

Verification date: 2026-10-27, Asia/Shanghai. The subject is the current playtest copy's 32-bit `Cube.exe`. This pass was research only: no mods installed, no drop rates adjusted, no running game invoked, and no saves read or modified.

## Conclusions for the player

This version's equipment acquisition is a stack of independent paths; there is no single master switch of "each boss randomly draws from four class equipments".

1. **Equipment the enemy is already wearing**: after the non-empty candidates are collected, one is drawn. Normally this triggers about one quarter of the time; with the `0x200` boss flag, that failure roll is skipped — if the candidates are non-empty, one drops. This "one quarter" is not your class's share.
2. **Extra random equipment**: entities matching the monster category trigger about one tenth of the time; it first splits weapon or armor, then draws one from the general candidate pools. This path does not favor the player's class.
3. **Valid quest-completion rewards**: when the quest and participation conditions are met, an extra piece filtered by the claiming character's class is generated, including universal jewelry; the local claiming path adds it straight to the bag. It is not re-issued for every arbitrary boss kill.
4. **Unidentified items**: entities matching another set of flags have their own drop condition; identification calls the general equipment pools and does not guarantee your class either.

So "fighting an off-class boss but getting your own rare gear" is entirely possible: both the extra random equipment and the quest reward can explain it. Conversely, being named Boss, being huge, or being hard to kill does not by itself prove it has droppable equipment.

What can give later read-only intel features a reliable basis is: **identifying the specific entity, checking the boss flag, and reading its currently worn candidate equipment, levels, and quality**. A fixed signature-drop table cannot be invented from the name alone.

## Verification method and scope

The machine instructions of the local program are authoritative; the recovered code is used only for locating things. The upstream recovery project is not the original author's source, and some function names and bodies are plainly wrong.

- `Cube.exe` SHA-256: `84A7A132A84D4282338E7EA45A1940D32066D64E39CBAFED8F2418D5A6DC30BF`.
- `MSVCR110.dll` SHA-256: `B30160E759115E24425B9BCDF606EF6EBCE4657487525EDE7F1AC40B90FF7E49`.
- Upstream CubeWorld-Reversal commit: `e6e906e4b2644806ef19c2e1f12ebc636c347f7c`.
- Not applicable to 2019 Steam or Omega; the dedicated server program and the multiplayer authority side were not separately verified this time.

Concrete verification:

| Item | Actual coverage |
|---|---|
| Original weapon and armor generation instructions | 10,000 input sets, covering the general pools and the four class pools |
| Every candidate at generation time | 70,000 field comparisons |
| Original quality-generation instructions | 120,000 input sets, comparing return values, RNG state, and consumption counts |
| Version rejection | A one-byte-modified isolated sample was rejected by the hash check; the copy instruction was never executed |
| Name prefixes | 5×10 keys recovered from the quality jump table and the seed jump table |
| Quality distribution | Analytic counting over all valid CRT states; see the probability section for the exact basis |

"Original-instruction comparison" ran audited function copies inside a separate research process. Container appends, item copies, allocation frees, random numbers, and the in-process secure-cookie helper calls were taken over by local stand-ins; no game DLL was loaded and nothing was injected into the game. It confirms the algorithms of these small functions but cannot replace full combat, quest settlement, and multiplayer testing.

Results are in native-checks.json (local evidence omitted from source publication) and analysis.json (local evidence omitted from source publication). The three read program/runtime files hashed identically before and after the research scripts ran; the scripts opened no saves. Save changes while the user plays the game on their own are outside this file-consistency statement.

## 1. The enemy's own equipment: look at what it wears first, then draw by slot

Entry `0x59B180`. Category 0, category 3, or entities with the `0x800` flag exit this death-loot function directly. Category 0 is the player; the full semantics of category 3 and the flags are not named presumptuously here.

Candidates are collected in the following order, accepting only items whose type byte is not 0:

| Entity offset | Normally corresponding item |
|---|---|
| `+0x990` | Off-hand |
| `+0xAA8` | Main hand |
| `+0x530` | Chestplate |
| `+0x878` | Pauldrons |
| `+0x760` | Gloves |
| `+0x648` | Boots |
| `+0x418` | Amulet |

These are the seven slots the program explicitly enumerates — **the two ring slots are not included**. An item's type, material, and other fields are still defined by the actual data.

The business condition can be written as:

```text
candidates = the items actually present in the seven slots above
if the candidates are non-empty and (rand()%4 == 0 or the flags contain 0x200):
    draw candidates[rand()%candidate_count]
    copy this existing item into the drop record
```

Here the item is not converted to the player's class and its quality is not re-rolled. When dual-wielding, both hand slots enter the pool separately; no boots worn means no phantom boot candidate.

Example: suppose a verified boss happens to have four candidates — main hand, chestplate, pauldrons, boots. This path drops one item, with each slot having a nominal chance of about one quarter; it is not "all four drop". A normal non-boss additionally has to pass the roughly one-in-four trigger first.

Evidence: death.asm.txt (local evidence omitted from source publication), focus `0x59B20E..0x59B422`. The recovered source misses this entire segment after collecting the candidates — the conclusion cannot be drawn from the pseudo-C alone.

## 2. Extra random equipment: ~10% trigger, and the four classes are not equally weighted

Inside the same death function, only category byte `+0x60 == 1` or species `+0x64 == 0x8F` enters this extra random check. When `rand()%10 == 0`, one random equipment piece is produced:

```text
base level = max(1, enemy level - 1 + rand()%2)
quality = roll_quality(enemy rank, no force-max)
weapon / armor = one binary choice
generate one piece from all class candidates
apply the vanilla ±0.05 curve jitter to the item level
```

The above describes the business condition only — **it omits the random calls for scatter position, orientation, and the like, and simplifies the call order; it cannot predict the next kill's result**.

### Weapon pool: 11 candidates, not every weapon equally weighted

| Class | Candidates | Generation rule per candidate |
|---|---:|---|
| Warrior | 3 | One-of-three among single-hand sword/axe/mace; one-of-three among two-hand sword/axe/mace; shield |
| Rogue | 3 | Dagger; longsword; fist weapon |
| Ranger | 2 | Bow; boomerang |
| Mage | 3 | Staff; wand; bracelet |

These 11 entries are built first, then one is drawn by candidate index. The warrior's "single-hand weapon candidate" is itself a three-way pick, so swords, axes, and maces must not be treated as separate pool entries with the same weight as the shield.

**This generator has no crossbow.** The original program consumes a random number before the ranger's second candidate but then sets the weapon type directly to boomerang. The enemies' own ranger weapon initialization does choose among bow, crossbow, and boomerang, so "a ranger enemy's own equipment with a crossbow" is a different source. This conclusion is not an assertion that shops, crafting, or other systems have no crossbows.

Evidence: weapon.asm.txt (local evidence omitted from source publication), `0x5F8B79..0x5F8D9C`; the ranger initialization in native-equipment.asm.txt (local evidence omitted from source publication).

### Armor/jewelry pool: 21 candidates

| Class / kind | Material | Candidates |
|---|---|---|
| Warrior | Iron | Pauldrons, chestplate, gloves, boots — 4 entries |
| Mage | Silk | Pauldrons, pauldrons, chestplate, gloves, boots — 5 entries |
| Ranger | Linen | Pauldrons, pauldrons, chestplate, gloves, boots — 5 entries |
| Rogue | Cotton | Pauldrons, pauldrons, chestplate, gloves, boots — 5 entries |
| Universal | Gold or silver | Amulet, ring — 2 entries |

The three cloth-armor groups' pauldrons are **actually appended twice**, each with a fresh seed; it is not a layout duplication in the report. There is no evidence this is an intentional balance choice by the author, and the motive is not explained for them.

Evidence: armor.asm.txt (local evidence omitted from source publication); the 21 append calls were verified one by one. When the same generator is restricted to a single class, the universal jewelry is still kept.

### After random equipment is generated, what is the class distribution

The following are the nominal weights computed with equally weighted candidates, ignoring CRT modulo bias and the state conditions for entering this channel; **they are not the total probability after killing an arbitrary boss**.

| Class | That class's exclusive weapon/armor | Plus the class-agnostic amulet/ring |
|---|---:|---:|
| Warrior | 23.16% | 27.92% |
| Ranger | 21.00% | 25.76% |
| Mage | 25.54% | 30.30% |
| Rogue | 25.54% | 30.30% |

The second column sums to about 95.24% over the four classes; the remaining ~4.76% is universal jewelry. "Passes the class restriction" also does not mean the level is high enough, the stats are suitable, or it beats your current gear.

## 3. Where quality colors and name prefixes really come from

The item's `+0x0C` is what this report calls quality `q`; the SDK calls the field modifier — the naming must not make it a separate quality system. `+0x04` is the seed, `+0x10` the internal level.

The full core algorithm of the quality generator `0x5F8530` is:

```python
q = rand() % (rank + 1)
q += (rand() % 100 == 0)
q += (rand() % 1000 == 0)
q += (rand() % 10000 == 0)
if force_max:
    q = rank + 1
return min(q, 4)
```

Even when `force_max` is true, the four random numbers are still consumed first, and the result is overwritten last. This too was checked against the original instructions.

The native color function `0x4C7D20` displays 0/1/2/3/4 as white, green, blue, purple, yellow. Higher values have other display handling and do not mean this generator normally produces higher tiers.

### The worn equipment's initial quality is not the same thing as the extra random equipment

The initial world-generation path `0x5ED407..0x5ED6BB` takes the `0x200` bit from the Spawn flags as `force_max`. So **in that initialization path**, a boss's equipment quality is `min(rank+1,4)`; ordinary entities draw each slot separately.

But quest bosses also have a flow that re-sets the level and rank and rebuilds weapons/armor — the initialization order must not be ignored. The verified flow at `0x5C7730` does not re-overwrite those slots' quality bytes. So you cannot claim, from a boss's current rank alone, exactly what color every piece it wears must be. Intel features should read the actual items directly.

Evidence: initial-quality assignment excerpt (local evidence omitted from source publication), quest re-initialization (local evidence omitted from source publication), quality function (local evidence omitted from source publication).

### "Shabby, intact, perfect" is not an extra durability bar

The actual name construction is at `0x598A50`, not the model-number function the recovered code mis-names as item_name_string_id. The name prefix is chosen by `q` and `seed % 10`; with `seed==0` the prefix is skipped.

| Quality color | Example vanilla keys |
|---|---|
| White | `shabby`, `battered`, `scratched`, `worn`, `common` |
| Green | `balanced`, `good`, `undamaged`, `flawless` |
| Blue | `exceptional`, `polished`, `exquisite`, `superb` |
| Purple | the second key group like `exceptional2`, `polished2` |
| Yellow | `brilliant`, `magnificent`, `legendary`, `fabulous` |

**The same color has several prefixes.** Each adjective the user sees must not be treated as an independent, strictly increasing gear tier. The vanilla keys are listed here; no claim is forced that a particular Chinese word in the current localization maps to a specific key. The full 50 entries, the two jump tables, and the addresses are in prefixes.json (local evidence omitted from source publication).

This corrects the earlier verbal simplification of "shabby swaps for shabby, perfect swaps for perfect". If a conversion is ever really made, at minimum the internal quality, level, and seed meanings must be preserved, and the legal mappings per weapon/material studied; names must not be swapped by the Chinese wording alone. No conversion is done in this pass.

## 4. Level, Power, and strength

The player UI's Power is not a direct read of the internal level; it is:

```text
C(L) = 1 - 1 / ((L-1)*0.05 + 1)
Power = int(C(L)*100 + 1)
```

The extra random equipment bases on the enemy's internal level `L-1` or `L` first, then jitters ±0.05 along this curve. It is not a fixed ±5 character levels.

| Enemy internal level | Enemy Power | Possible equipment internal-level range on this random channel |
|---:|---:|---:|
| 10 | 32 | 7–12 |
| 20 | 49 | 15–24 |
| 30 | 60 | 23–36 |
| 50 | 72 | 39–64 |
| 100 | 84 | 72–150 |

The ranges come from a model stepping the original instructions in single precision; they do not mean every piece in the range suits the player. The curve flattens at high levels, so the internal-level jitter range widens. The enemy's already-worn equipment is copied directly, without this death-time level jitter; the quest-reward and identification paths also cannot directly reuse this range.

The general base-stat scaling function `0x445F60` is `2^(3*C(L)) * 2^(q/4)`, with each piece further multiplied by slot-, weapon-type-, material-, and seed-related factors. That explains why level, color, and the concrete stats must be read together when comparing upgrades. "Higher level" must not be equated with "every stat necessarily stronger".

Evidence: level-jitter.asm.txt (local evidence omitted from source publication), level-curve.asm.txt (local evidence omitted from source publication), stat-scaling.asm.txt (local evidence omitted from source publication).

## 5. Quest rewards: filtered by the player's class, but the quest must actually be validly completed

The kill-settlement path verified here queries the enemy's place's quest record, checks that the quest is not yet complete, and confirms the target flag/quest conditions against the actual participation records. Some quest types complete immediately; others complete after accumulating kills; this quest-progress counter is not a "consecutive drop-failure" pity counter.

After a valid completion, inside the settling loop for participating players:

1. A platinum-coin reward record is generated.
2. The weapon or armor generator is called as a two-way choice, the fourth parameter explicitly passing **the claiming character's class at `+0x140`**.
3. The level and quality use the parameters carried by the quest record; the death-time random equipment's quality draw and level jitter are not re-called here.
4. When the claiming party is the local player, the bag-add function is called. So not all quest rewards are found by staring at the ground.

Evidence: mission.asm.txt (local evidence omitted from source publication), `0x596439..0x596525`, `0x5968FA..0x596AD8`, especially the character-class read at `0x596A39`. The generator itself still allows universal jewelry, so "filtered by your class" does not equal "guaranteed a weapon of your class".

This reward targets quest completion; it is not issued on every respawn of every boss. Without the concrete kill scene and quest state, it is impossible to retroactively assert whether the user should have gotten this reward on some occasion.

## 6. Unidentified items, recipes, pet food, and other branches

| Branch | Verified conditions and results |
|---|---|
| Recipe scroll | When the monster category above matches, another `rand()%50==0`; the quality function takes the enemy's rank + 1 as input, and the generated equipment description is wrapped as item type 2. It is not an unidentified item. |
| Pet food | After the extra random equipment occurs, another `rand()%5==0` generates pet food of type `0x14`, count 1. ~2% is the nominal product of the two layers; the food kind is chosen from the controller's list, not by the player's class. |
| Unidentified item | After entering the separate `(flags & 0x18)!=0` branch, the quality is drawn first, then a ~5% check; zero quality drops nothing, generating item type `0x0E`. The full semantics of these flags are kept as-is — they are not lumped into some single boss kind. |
| In-inventory items | A category-1 entity's death also walks its own item list, dropping non-empty items by count, but explicitly skipping health potions (type 1, subtype 1). This is separate from the seven wearable-slot draw. |
| Species-specific materials | The function also branches by species to generate vegetables and the like; materials must not be miscounted as weapon/armor drops. |

The unidentified item's quality branch is specifically: `v=rand()%1000 + rank*20`; `v<700` gives 0, `700..949` gives 1, `950..998` gives 2, `>=999` gives 3; with the `0x200` flag it gains one more, capped at 3. Then it requires `rand()%20==0` and a non-zero quality. The level is the enemy's level.

On identification, the item's level and quality are read, then the general weapon or armor generator is called with the class parameter `-1`; so identification does not guarantee your class, and this weapon generator likewise has no crossbow candidate.

Evidence: death.asm.txt (local evidence omitted from source publication) `0x59BB28..0x59C163`; formula.asm.txt (local evidence omitted from source publication); pet-food.asm.txt (local evidence omitted from source publication); identify.asm.txt (local evidence omitted from source publication).

## 7. Probability basis: rand() must not be treated as an independent, infinite-precision die

The local CRT is: `state = state*214013 + 2531011`, returning `(state>>16)&0x7FFF`. Each output is only 0..32767; e.g. `%100==0` corresponds to 328 outputs — about 1.00098% under uniform single outputs, not a mathematically exact 1%. Consecutive outputs are also correlated.

This pass did an analytic count over all `2^31=2,147,483,648` valid states of the quality function: the top bit of the original 32-bit state does not affect the low 31-bit chain of later returns, so duplicated top bits are folded away. The implementation enumerates the union of the three rarity-bonus events and adjusts from each rank's baseline distribution; the game function was not run two billion times.

| Quality function input rank | White | Green | Blue | Purple | Yellow |
|---:|---:|---:|---:|---:|---:|
| 0 | 98.887252% | 1.111605% | 0.001143% | 0 | 0 |
| 1 | 49.443580% | 49.999520% | 0.556329% | 0.000571% | 0 |
| 2 | 32.963426% | 33.333965% | 33.331334% | 0.370895% | 0.000381% |
| 3 | 24.721779% | 24.999839% | 24.999887% | 24.999977% | 0.278518% |
| 4 | 19.778664% | 20.000984% | 20.001214% | 19.998214% | 20.220925% |

**This table assumes all valid CRT states are equally likely when entering this one function.** It is not a measured kill rate, it excludes the conditions for entering the drop channels, and it does not apply to force_max overwriting the output. A mechanical ×10% must not be multiplied on top to fake a precise total drop rate for a specific boss.

One technicality: three consecutive returns from the local CRT cannot satisfy all three bonus conditions at once — the analytic count is zero. The three checks in the code must not be treated as mutually independent, fabricating a tiny non-zero purple chance for rank 0 in this function. This conclusion is limited to this CRT and this call order.

## 8. How to pick targets worth fighting

Derived from the confirmed mechanics, the priority should be:

1. Check whether the target actually has worn candidates and carries the flag that makes its own equipment drop guaranteed.
2. Check the candidates' own level, color, slot, and class restriction; a matching class is a filtering clue, not a substitute for the actual item data.
3. Check whether the current quest is valid and unfinished, since the quest reward is filtered by the claimer's class.
4. Keep expectations for the other extra random drops, but do not treat them as a promise of guaranteed upgrades.

For example, wanting a crossbow: a ranger target already holding a crossbow is better-grounded than relying on the general weapon generator verified here; wanting a higher-quality staff, focus on the boss equipment actually holding staves, not just the place name, the size, or the enemy name's color.

In the death, random-generation, identification, and quest-reward paths verified here, no logic was found that fills the player bag's gaps, weights by consecutive failures, or prioritizes the player's class in the general random drops. The quest progress, the boss's guaranteed own-equipment drop, and the normal recipes must not be confused with the nonexistent "class-failure pity".

## 9. What still cannot be claimed

- No specific user's actually killed bosses were tracked, so all their historical empty drops cannot be explained; no in-game statistical sample was made.
- Shops, crafting, chests, ground generation, and the dedicated server's full acquisition chains are outside this report's "boss death and quest rewards" scope.
- Reading far-away, not-yet-loaded boss equipment is not guaranteed, and the next actual random result is not predicted.
- The full names of the original flags like `0x18` and the final display mapping of every localization prefix are not yet fully verified; the report keeps the original values and keys.
- Some special bosses have different generation branches. The initial-generation formulas cannot cover quest rebuilds or a specific entity's actual fields.

## Reproduction and evidence entry points

Rebuild the offline probe (with the installed Visual Studio x86 build environment):

```bat
C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsall.bat x86
cl /nologo /EHsc /O2 /MT /std:c++17 /Fe:build\loot_native_probe.exe /Fo:build\loot_native_probe.obj research\loot_native_probe.cpp
```

Re-extract the local instructions, verify the generators and quality function, and generate the analysis JSON:

```powershell
python -X utf8 research/analyze_loot.py --disassemble --native
```

Sources: [loot_native_probe.cpp](loot_native_probe.cpp), [analyze_loot.py](analyze_loot.py). All outputs are pinned to `research/evidence/loot-20261027/` and write nothing into the game directory. A mismatched game file is rejected before any instruction is copied and executed.

Key address map:

| Address | Use in this verification | Misleading upstream name / notes |
|---|---|---|
| `0x59B180` | Main death-loot flow | The recovered body misses the own-equipment draw branch |
| `0x5F51E0` | Random armor/jewelry | Mis-named buildDecalMesh |
| `0x5F8AD0` | Random weapons | Mis-named buildDecalMesh2 |
| `0x5F8530` | Quality generation | Mis-named vectorElementAt |
| `0x5F6D50` | Recipe wrapping | Mis-named emitDecalOrParticle |
| `0x5F79E0` | Pet food generation | Mis-named as a plain memset init |
| `0x4C7E80` | Internal level-curve jitter | Mis-named rerollStackCount |
| `0x4C76A0` | Displayed Power | Mis-named rarityScaled |
| `0x598A50` | Actual item name construction | Prefixes recovered from two jump tables |
| `0x51BE60` | Weapon model/icon number | Not the item display-name construction |

This pass only added research tools, the report, and the evidence; no drop mod was generated, and no existing manager, loader, or mod was changed.
