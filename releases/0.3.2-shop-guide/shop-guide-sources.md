# 2013 Alpha Shop Guide: Research Notes and Copy

Verification date: 2026-10-23. The goal was to add readable Chinese purpose hints to the existing shop signs. The Chinese display names follow the mod's existing translations; the table below keeps the English names from the references and does not treat the translated names as separate establishments.

## Per-category sources

| Mod display name | Reference name | Verified use and scope adopted here | Sources |
| --- | --- | --- | --- |
| 旅馆 (Inn) | Inn / Innkeeper | In Alpha, talking to the innkeeper advances time to 7:00 AM for free; the July 2013 update limited the reset to night hours. The UI says "at night", not "anytime", and does not copy Steam's paid lodging. | [Time](https://cubeworld.fandom.com/wiki/Time), [2013-07-23 update](https://cubeworld.fandom.com/wiki/July_23rd%2C_2013) |
| 道具店 (Item shop) | Item Shop | Alpha shops can sell materials, pet food, recipes, and key items; 2013 starter guides explicitly say empty bottles, gliders, and boats are bought at the general store. It lists common categories and does not promise every shop currently stocks them. | [Item Shop](https://cubeworld.fandom.com/wiki/Item_Shop), [Trade District](https://cubeworld.fandom.com/wiki/Trade_District), [2013 starter guide](https://game.entertainment14.com/2013/07/cube-world-alpha.html) |
| 武器店 (Weapon shop) | Weapon Shop / Weapon Vendor | Alpha sells weapons for every class; swords, daggers, bows, and staves are given as examples of weapon types. General shops also buy items. | [Weapon Vendor](https://cubeworld.fandom.com/wiki/Weapon_Vendor), [Weapons](https://cubeworld.fandom.com/wiki/Weapons), [Shops](https://cubeworld.fandom.com/wiki/Shops) |
| 防具店 (Armor shop) | Armor Shop / Armor Vendor | Alpha sells armor for every class; chestplates, gloves, boots, and pauldrons are given as examples of armor slots. General shops also buy items. | [Armor Vendor](https://cubeworld.fandom.com/wiki/Armor_Vendor), [Cotton Armor slot table](https://cubeworld.fandom.com/wiki/Cotton_Armor), [Shops](https://cubeworld.fandom.com/wiki/Shops) |
| 鉴定店 (Identifier) | Identifier's Shop | Identifies Leftovers (unidentified items) for a fee, usually revealing a weapon or armor. Only the identification service is described; it is not described as selling ordinary equipment. To avoid clashing with local translation variants, the notes avoid the unverified "残余物/遗留物" renderings. | [Identifier's Shop](https://cubeworld.fandom.com/wiki/Identifier%27s_Shop), [Leftovers](https://cubeworld.fandom.com/wiki/Leftovers), [2013 Alpha guide](https://www.gamestar.de/artikel/cube-world-alpha-guide-einstiegshilfen-tipps-tricks-und-loesungen%2C3026211%2Cseite4.html) |
| 铁匠铺 (Smithy) | Smithy | Provides processing facilities like the furnace and anvil; the customization bench nearby adds material cubes to weapons. It is described as processing and crafting, not as a shop that directly sells finished goods. | [Smithy](https://cubeworld.fandom.com/wiki/Smithy), [Customization Bench](https://cubeworld.fandom.com/wiki/Customization_Bench) |
| 木工坊 (Workshop) | Workshop / Woodworker | The saw processes wood; the workbench crafts wooden weapons, including bows, crossbows, and staves. | [Saw](https://cubeworld.fandom.com/wiki/Saw), [Workbench](https://cubeworld.fandom.com/wiki/Workbench), [2013 Alpha guide](https://www.gamestar.de/artikel/cube-world-alpha-guide-einstiegshilfen-tipps-tricks-und-loesungen%2C3026211%2Cseite4.html) |
| 纺织坊 (Clothier) | Clothier / Tailor | The spinning wheel processes yarn; the loom crafts cloth armor, using cotton, linen, silk, and similar materials. | [Spinning Wheel](https://cubeworld.fandom.com/wiki/Spinning_Wheel), [Crafting](https://cubeworld.fandom.com/wiki/Crafting), [2013 Alpha guide](https://www.gamestar.de/artikel/cube-world-alpha-guide-einstiegshilfen-tipps-tricks-und-loesungen%2C3026211%2Cseite4.html) |

Some direct Fandom page fetches were blocked by the site; this pass used the entry content the web search tool actually returned, cross-checked against guides explicitly published in 2013. No claim is made that unreturned content of blocked pages was read. The inn's night condition also matches the local reverse-engineering reference `GameController.cpp` around line 96883 (the 18:00/06:00 check and the 7:00 assignment).

Out of scope: Steam's supply-gnome unlocks, flight administrators, regional equipment restrictions, paid overnight stays, buyback pages, and books of crafting. Suggestions and common goods were not written up as reads of current stock.

The two-line in-game blurbs are independent Chinese write-ups; the source lives in `navigation/town_guide.hpp`. That file is authoritative for the content. Web references back the purposes; the original game's entity data backs the shop name types and coordinates; the two kinds of evidence are used separately.

## Sidebar design

The existing palette is kept: ink #14181E, off-white border #ECF1E0, deep blue #223A4D, gold #FFD86A, body cream #FFF6D8, secondary text #BCD3D2. The original FZCuYuan font is kept; shop names at 15 and body text at 12.5 design units, scaling with the existing ratios.

A single "shop guide" panel top-right, left-aligned: each row gives the shop name and distance first, then the two purpose lines. The original map sign names are kept; the purpose text goes only into the sidebar, and the whole guide hides when the map is closed. Only nearby discovered shops are listed; undiscovered shops are not passed off as existing local places.

The existing pixel border is used, with no added images, animations, or decorations. The sidebar is 336 wide with a 62-unit row height. The standalone preview checks all 8 categories, including an 800×480 small window; the preview is illustrative data, and real in-game verification is recorded separately.

Illustrative structure:

```text
店铺指南
武器店                         166 m
购买各职业武器，也可出售杂物。
常见类型：剑、匕首、弓、法杖等。

右键导航，商品以店内库存为准
```
