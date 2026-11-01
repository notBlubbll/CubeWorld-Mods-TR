# Launcher icon

`launcher-icon.png` is the generated original with a transparent background; `launcher.ico` contains 32-bit layers of 16, 20, 24, 32, 40, 48, 64, 128, and 256 pixels, for Windows programs, the taskbar, and shortcuts.

Generated 2026-10-24 with the built-in imagegen tool, after first reading and actually viewing the Picroma official-site references:

- [Game website](https://picroma.com/)
- [Character design reference](https://picroma.com/media/warrior.jpg): a blond, blue-eyed, big-headed short-bodied voxel adventurer with dark shoulder armor and a back-mounted sword.
- [Logo color reference](https://picroma.com/media/cube-world-logo.png): warm orange-gold pixel highlights.

The reference images live only in the ignored `build/icon-reference/` and are not distributed as program resources. The first grass-block draft was not adopted. The new icon is used only for this project's mod manager; it replaces neither the original game nor the shared launcher.

The blond adventurer original was restored at the user's request. The ICO is built by `Build-Icon.ps1` using .NET System.Drawing, scaling with the alpha channel preserved and then packaging into 32-bit DIB layers — avoiding the garbled output .NET Framework produces when scaling PNG layers by drawing. The original PNG stays unchanged. When updating the icon, run `Build-Icon.ps1` in this directory first, then the root `Build-ModManager.ps1`; the build embeds the same ICO for the program and window and updates the local shortcut.

The generation prompt follows:

```text
Use case: logo-brand. Asset: one finished transparent Windows desktop app icon for a personal CUBE WORLD mod launcher. Reference image 1 (official-warrior.jpg) is the key visual-style and character-proportion reference, NOT an edit target: study its actual Picroma Cube World character, oversized cubic head, small voxel subdivisions, blonde chunky hair, large bright blue eyes, peach block face, tiny dark shoulder armor, and sword. Reference image 2 (official-logo.png) provides the warm orange/gold accent palette only; do not draw its lettering. Create a NEW tightly composed, charming three-quarter FRONT VIEW HEAD-AND-SHOULDERS PORTRAIT emblem of a blonde blue-eyed Cube World fantasy adventurer, matching the distinctive cute proportions and detailed small-voxel construction of reference 1. Big readable friendly face and blocky swept blonde hair are the focal point. Compact navy/charcoal shoulder plates, a small warm orange-gold square clasp at the chest, a single simple steel sword hilt and short visible blade silhouette emerging diagonally behind one shoulder. Pose is confident and friendly, gaze toward viewer. Head is large and readable; very short neck, compact shoulders, no elongated realistic torso. Hard cubic contours, simple solid shaded voxel faces and tiny stepped hair silhouette, bright playful classic 2013 Cube World RPG inventory-portrait aesthetic. A restrained dark contour helps against light Windows desktops. NOT a Minecraft character or Minecraft terrain icon: no grass/dirt block, no floating landscape, no pickaxe, no Steve, no generic isometric cube. Isolated centered bust, fills about 80% of square canvas, even transparent padding, no background tile or medallion, no scenery. True transparent RGBA outside the portrait. Very strong clean silhouette at 32x32, 48x48; limit fine detail. No lettering, logos, watermark, UI screenshot, variants sheet, glossy clay render, soft round shapes, thick cartoon linework, sparkle effects, or photorealism. One icon only.
```
