# First Source Release Validation

Date: 2026-10-31. The subject is this public source copy — not the formal game directory and not the original development directory.

- Using the dependency sources kept in the repository, the Visual Studio 2022 / Win32 / Release configuration and the default targets were built successfully in a fresh `build/`. The standalone mods, the shared loader, and the launcher all built.
- `Build-ModManager.ps1` successfully produced the Windows mod manager and the local shortcut inside the copy. The original manager entry points were not modified.
- The 26 CTest cases from the command in `BUILDING.md` all passed. The normal mode of `inventory_module_checks`, which depends on four unpublished live inventory samples, was not run; that live replay is not counted as passing.
- The release file inventory, the third-party licenses, the common key formats, and the local private identifiers were checked. The game itself, saves, asset packs, fonts, live data, logs, and build outputs were not added.
- The gameplay modules and the shared framework keep their original logic. The local paths in the public copy were replaced with unified example directories; the matching target paths, source verification, and manager displays were updated in sync, and the strict verification remains. The previews and offline fixtures use synthetic identifiers; personal install and compensation records are not public.
- After the privacy cleanup, the manager, the travel checks, the compensation tools, the restricted QA module, and the previewers were recompiled, and all 26 offline checks passed again; the syntax check of the 26 PowerShell scripts reported no errors, and the Python syntax and JSON structure checks passed. No QA module was installed or run, and the game was not started.

This was source packaging, compiling, and offline checking; the game was never started, and this does not replace real in-game acceptance or compatibility testing of every community mod. For the manager's example directories, fixed hashes, and the current installation restrictions, see `BUILDING.md`.
