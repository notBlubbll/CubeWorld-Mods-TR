# Building and Current Limitations

## Environment

- Windows, Visual Studio 2022 with the "Desktop development with C++" workload, the MSVC x86 build tools, and the Windows SDK.
- CMake 3.20 or later. The project uses Win32; compiling the game mods as x64 is not supported.
- The manager uses the .NET Framework 4.x C# compiler that ships with Windows; npm is not used.
- The manager currently prefers PowerShell 7; when it cannot be found, it falls back to read-only queries via Windows PowerShell 5.1 and locks write operations.

The dependency sources are kept in the repository under `external/Qube-Loader/`. This repository stores the pinned build dependencies as plain files, so no submodule download commands are needed; please keep the licenses and the `LOCAL_CHANGES` record in that directory.

## Building the C++ mods

From the repository root, in a PowerShell that can find CMake, run:

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A Win32 -DCMAKE_TRY_COMPILE_CONFIGURATION=Release
cmake --build build --config Release --parallel 1 -- /nodeReuse:false
ctest --test-dir build -C Release --output-on-failure -E "^inventory_module_checks$"
```

The artifacts land in `build/bin/`. Single mods can be built separately with `--target waypoint`, `--target potion_shop`, and so on. The shared framework and launcher targets are `cube_mod` and `NavigationLauncher`.

The normal mode of `inventory_module_checks` reads four inventory binary samples captured on the maintainer's machine. Those samples are not published, so the command above explicitly excludes that live replay; its source is still kept, and the other inventory anomaly modes that need no samples still run. Do not describe this exclusion as that replay having passed.

The `Build-*.ps1` scripts in the repository keep the original build flow, with the CMake paths replaced by example paths for a common Visual Studio install directory. On other machines, prefer the standard CMake commands above instead of copying the example absolute paths.

## Building the manager

```powershell
.\Build-ModManager.ps1
```

This produces `build/bin/ModManager.exe` plus the local shortcut inside the repository. Building does not start the game.

## The install flow is not yet generalized

In the public copy, the game directories used by the management and install scripts are unified to the example `C:\Games\CubeWorld`, and the example project path is `C:\Projects\CubeWorld-Mods`; neither represents the maintainer's real directories. The playtest copy lives in `playtest/` inside the repository; source paths marked for the playtest copy are still strictly verified. The original installer reads pinned DLL versions and their hashes from `releases/`, while the source repository only contains the text notes and the necessary metadata — not those prebuilt files.

A loader or launcher you rebuild yourself will not necessarily match the hard-coded SHA-256 values, and the manager may lock itself accordingly. **Do not remove the hash checks, ignore conflicts, or flip a version-mismatch switch to force it to run.** A public installer package additionally requires path configuration, a trusted build manifest, and install acceptance before matching binaries can be published.

So this source release is meant for reading, research, building, and offline checks. It is not a promise of a finished product that installs with a double-click on any computer, and it does not require placing the game at the same disk path as the maintainer.

The module docs keep the necessary technical history; specific personal gameplay and install records have been removed. The example paths and local verification records in the historical packages do not mean the public repository contains the matching attachments; install state is defined by the actual files and the safety checks.
