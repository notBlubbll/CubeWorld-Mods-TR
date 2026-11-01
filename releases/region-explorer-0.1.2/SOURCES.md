# Region Explorer: sources and verifiable boundaries

Verification time: 2026-10-24. Only this machine's verified 2013 Alpha is supported: `Cube.exe` SHA-256 `84A7A132A84D4282338E7EA45A1940D32066D64E39CBAFED8F2418D5A6DC30BF`.

- [RegionReveal](https://github.com/Jovinull/RegionReveal/tree/8633915f10b6ad565a31fc653d4f7e3f3b2c2484), pinned commit `8633915f10b6ad565a31fc653d4f7e3f3b2c2484`. Its `docs/MAP_LABELS.md`, `docs/REVERSE_ENGINEERING.md`, `docs/BEHAVIOUR.md`, `src/game/cube_world.hpp`, `src/game/world.cpp`, and `src/region_reveal/marks.cpp` were referenced for this version's map layout, native label categories, and quest states. The original MIT license is kept at `RegionReveal-LICENSE.txt`. This module implements its own Qube SDK read-only scan, list, target storage, and optional navigation messaging; the project's dinput8 proxy and native patch were not installed, and its region-reveal functions were never called.
- [CubeWorld-Reversal](https://github.com/qad3n/CubeWorld-Reversal): the local `external/CubeWorld-Reversal/cube/control/GameController.cpp`'s `NameGen_initFirstNameTables` (original address 0x5a0ed0) and the region-generation calls; the map serialization function in the local `cube/_library/crt_stl.cpp` (original address 0x605420). The map save structure differs from live quest data — a missing quest field in a save must not be read as "no boss".
- [Qube-Loader](https://github.com/qad3n/Qube-Loader): the project's existing SDK, ImGui, and shared loader are reused without changing the shared framework. Its GPL-3.0 license is kept with the project.
- Local read-only evidence: `research/evidence/explorer-name-layout.json` verified the 40 UTF-16 name syllables in the actual executable; `explorer-saved-records.json` verified two real maps, 761 landmarks, and 24 towns among them. The test records rebuild only the fields the save already has — no live quest states are fabricated.

WorldMap is the controller-embedded object at `gc+0x800d44`; the sparse block table is at `+0xb0`; each block's landmark area is offset 0x34000, holding 64 records of 0x68 bytes each. Town is category 1; the quest ID is at record +0x34 and the state byte at +0x41 (0 unfinished, 1 in progress, 2 completed). Categories 0/10 take no part in native landmark labels; the other categories not yet fully verified are just called "landmark".

Names use the native default species-index algorithm, keeping the uint32 overflow: `(seed/7 + seed*13)%20`, `(seed*7 + seed/10)%20`. The source stores only the RVAs of the strings in this version and reads them from the user's original program at runtime — no game fonts, word lists, or map assets are distributed. No Chinese proper-name conversion was implemented.

Storage blocks are fixed-size technical chunks; the borders of named geographic regions are decided by other noise/partition rules, so a storage-block edge is not a desert or snow-region boundary. This version draws no such borders and implements no offline world prediction for arbitrary seeds.

## 0.1.2's vanilla region difficulty colors

The pinned commit's `docs/MAP_LABELS.md` was fetched over the GitHub connection; the local reference copy is at `research/upstream/RegionReveal/MAP_LABELS.md`; that document clearly separates region-name colors from quest-completion marks. The local client with the exact SHA256 was then disassembled byte by byte — no original game function was executed directly.

- The vanilla map labels at `0x4CAC69..0x4CADB3` read the region record's level at `+0x24` and the local character's at `+0x190`, calling `0x43CA60` for each.
- `0x43CA60`'s single-precision curve is `C(L)=1-1/((L-1)*0.05+1)`. The original constant `0x745DC0` is `1.0f`, `0x6FCDB4` is `0.05f`; the threshold `0x6FCB7C` is `0.1f`.
- Region curve values `<= C(player)-0.1f` stay white; above that line and `<= C(player)+0.1f` are cyan RGB `(0,1,1)`; higher are red `(1,0.2,0.2)`. The city branch stays plain white. The equality on both sides and the float evaluation order follow the original program.
- This is the region-level color using `+0x24` — never the quest-monster level `+0x38` or the quest completion state — and it computes no equipment power. Distance plays no part in the color.
- This module implements this pure computation itself — it does not call or hook the native coloring function and writes no game state; it only adds one clearly-successful/failing 4-byte character-level read while the map is open; the scan frequency and range are unchanged.

The constants and layout are in `research/evidence/explorer-difficulty-native-rule.json`; the map branch and curve disassembly are in `explorer-difficulty-map-color-disassembly.txt` and `explorer-difficulty-native-curve-disassembly.txt`.
