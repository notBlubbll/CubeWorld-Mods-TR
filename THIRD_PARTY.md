# Third-Party Sources and Licenses

This source release was assembled on 2026-10-31. Pinned versions are defined by `SOURCE_SNAPSHOT.json`.

| Component | Upstream and pinned version | Bundled license / handling |
| --- | --- | --- |
| Qube-Loader | [qad3n/Qube-Loader](https://github.com/qad3n/Qube-Loader), `4c3dad5a4665ffcc40117aef7fe6da6fdea61ca9` | [GNU GPLv3](external/Qube-Loader/LICENSE). The sources required for the build are kept, including the two existing local changes — see the [change record](external/Qube-Loader/LOCAL_CHANGES.md). |
| Dear ImGui | [ocornut/imgui](https://github.com/ocornut/imgui), `8936b58fe26e8c3da834b8f60b06511d537b4c63` | [MIT](external/Qube-Loader/modloader/sdk/imgui/LICENSE.txt). The core and the Win32/DX9 backends are kept; the example fonts are not distributed. |
| MinHook | [TsudaKageyu/minhook](https://github.com/TsudaKageyu/minhook), `c3fcafdc10146beb5919319d0683e44e3c30d537` | [Original BSD-style license with attribution](external/Qube-Loader/modloader/include/minhook/LICENSE.txt). |
| RegionReveal | [Jovinull/RegionReveal](https://github.com/Jovinull/RegionReveal/tree/8633915f10b6ad565a31fc653d4f7e3f3b2c2484) | Used as a reference for map layout and analysis material; its [MIT license](region_explorer/RegionReveal-LICENSE.txt) is kept, see `region_explorer/SOURCES.md` for details. Its dinput8 proxy and reveal patch were not installed. |

## Upstreams used for research only

- [CubeWorld-Reversal](https://github.com/qad3n/CubeWorld-Reversal): used to help understand layouts, calls, and offsets, cross-checked against the local client's instructions. That recovered-source project was not copied wholesale into the public repository and is not a build dependency.
- [Synap5e / Trader Fast Travel](https://github.com/synap5e/cubeworld-trader-fast-travel), research commit `27e57f113b0aa1bbb4c78c219a5ab6fac8c6a12c`: referenced for the travel flow and coordinate handling. The local development records could not confirm an upstream open-source license, so that project's original sources/binaries are not copied or relicensed with this repository. For the current implementation boundary see `fast_travel/SOURCES.md`; do not add its original research copy to the release scope.

## Icons, fonts, and game assets

- The manager's avatar is a voxel-adventurer icon generated for this project; the generation notes, reference sources, and PNG/ICO are kept — see the [icon notes](mod_manager/assets/README.md). The generated image is not relicensed as third-party program code.
- The original Picroma reference images are not part of the source release. The game itself, asset packs, word lists, and system fonts are not bundled. Code reads assets from the user's machine on demand.
- Game names, trademarks, and related rights mentioned in the mod docs belong to their respective rights holders; this project does not represent Picroma.
