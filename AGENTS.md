# Cube World Mod Project Conventions

## Taking over and scope

- When starting work, first read the root `README.md`, `MODS.md`, and the docs of the feature directories involved, and check Git status. Existing uncommitted changes are the current working scene; do not overwrite them or sweep them into your commits.
- The user wants features such as the map and the potion shop to be independently installable, toggleable, and removable. New features inherit this boundary by default; do not merge them into one big mod just because they share the project directory.
- When adding a standalone mod, wire it into the existing visual mod manager by default. That is part of the mod's delivery scope; the user does not need to ask for it again or remind in another conversation. A mod without manager integration must not be declared delivered. See "Default delivery requirements for new mods" in `MODS.md`.
- Refactoring a module must not change gameplay along the way — potion prices, progression rules, map interaction, and the like.

## Reuse check before starting new work

- Before adding a mod or significantly extending existing gameplay, first read the [current development direction](research/development-direction-2026-10-29.md), then check the latest official Omega patches and existing mods as needed. The [2026-10-25 comparison](research/omega-mod-planning-2026-10-25.md) is a historical snapshot; its dated statements must be refreshed and must not be treated as the latest conclusion.
- For direction calls, first check the related research, working records, and the necessary past project conversations; reuse choices the user has already made instead of making them restate known information. For equipment-rolling questions, read the [2013 loot reverse-engineering report](research/loot-mechanics-2013-2026-10-27.md) first; the current priority is real equipment intel for vanilla bosses, while drop rates, class switching, and pity rules remain unselected gameplay-rework proposals.
- Before starting, briefly remind the user: the current pain point, whether an official or existing solution already solves it, whether it can be used directly in the current version, and the reasoning behind "do it / can wait / don't invest yet". When a usable solution exists, prefer reuse; presence in a repository does not mean compatibility — reuse still requires checking license, version, framework, save data, and conflict boundaries.
- Distinguish what officials have actually shipped, what is a dev demo or a plan, player reports, and unknowns. A similar Omega feature does not automatically negate a low-cost 2013 fix, nor does it prove that local DLLs, offsets, saves, or configs can migrate directly.
- For the late-game lack of goals, first verify that a small-scale goal or logging scheme actually increases the desire to keep playing, before considering new reward systems, camps, villagers, or economy systems; do not treat more numbers, location lists, or repeatable quests as long-term gameplay improvements.
- This step exists to keep trade-offs in mind, not to add repeated confirmations to ordinary fixes. Once the user has clearly chosen to continue with the current version or accept the differences, proceed as authorized; do not cancel tasks on your own because of official future plans, and do not automatically install new tools, switch game versions, or enable periodic monitoring.

## Modules and the shared framework

- The shared parts are Qube Loader and the launcher. The map is `waypoint.dll`, the potion shop is `potion_shop.dll`; neither may be made a required condition for the other to start or install.
- The current town labels and shop guide still belong to the map module; do not claim they are a standalone DLL. Whether to split them further depends on actual need.
- Each new mod uses a unique DLL name and manifest ID; its config, data, and toggle keys use their own names. Existing data directories are isolated by DLL file name; do not rename them casually, which would orphan old data.
- Install, update, and management scripts only touch the files and config keys owned by the selected mod and preserve the other mods' content. Shared-framework upgrades are handled separately and must not be bundled into ordinary feature updates.
- Prefer the existing loader's interfaces. Before changing the same game function, price, inventory, or memory location, check how other mods use it; load order and priority do not substitute for compatibility verification.
- When a check fails, refuse or disable explicitly; never force it to run by disabling version checks, hard-patching the game program, or ignoring conflicts.

## Install/removal and data protection

- Install and removal count only after the target game and launcher have fully exited; do not implement or promise unverified hot-plugging while the game is running.
- Prefer `Manage-Mod.ps1` for single-mod management. Disabling keeps the DLL; removal only moves the selected DLL into the project backup and deletes nothing.
- By default, keep the original character and world saves, each mod's own data, and its config. Disabling the potion shop does not roll back purchases, consumption, or game states that were previously saved normally.
- Removing one feature keeps the shared loader, the launcher, and the other mods. While the old launcher still requires the map DLL, disable the map first; refuse to remove the map directly until the shared launcher has been upgraded.
- Operate on the playtest copy by default. Installing to or configuring the formal directory requires the user's explicit authorization for that target; never start the game automatically.

## Validation and delivery

- A new mod must ship, in the same delivery, with its manager name, description, install detection, independent switch, and safe lockout; keep the status interface, management scripts, the necessary build/install entry points, and docs in sync, and rebuild the manager. Producing only a DLL or only changing source does not count as full integration.
- When updating an existing mod, re-check that its name, path, and enable/disable flow in the manager still match; keep the original switch and do not add duplicate entries. Only when the user explicitly asks for research-only, prototype-only, or no-integration do you deliver to that narrower scope.
- When changing a single module, check it running alone, running with the existing mods, and how the other mods behave after it is disabled or removed. Record offline checks and in-game acceptance separately; do not treat source independence as all combinations being compatible.
- The combinations currently needing attention are: map only, potion shop only, both together, both disabled, and restarts after removing one of them.
- Protect existing uncommitted work; do not create extra Git worktrees without authorization, and do not sweep other tasks' changes into commits. Handle Git staging and commits per the user's current authorization.
