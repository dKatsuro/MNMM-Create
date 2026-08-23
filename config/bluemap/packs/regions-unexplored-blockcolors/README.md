# Regions Unexplored block tints for BlueMap

Hand-authored here; no upstream pack exists for this (the community BlueMap
packs cover vanilla or other mods). Copy the whole folder into
`config/bluemap/packs/` on the VPS -- BlueMap accepts a folder or a zip.

## The problem

Regions Unexplored registers its block tints **in code**, via
`RegisterColorHandlersEvent$Block` in
`net.regions_unexplored.client.color.RuColors`. That is client-side Java, not
data, so BlueMap cannot see any of it.

BlueMap's own `assets/minecraft/blockColors.json` opens with:

```json
"default": "@foliage"
```

So every RU block whose model carries a `tintindex` -- directly or inherited
from `minecraft:block/leaves` or `minecraft:block/tinted_cross` -- gets painted
with the biome **foliage** colour. 82 RU blocks inherit a tint that way.

## What this file changes

58 entries. The other 24 need no entry:

| Group | Count | Entry | Why |
|---|---|---|---|
| Grass-tinted | 16 | `@grass` | `RuColors` gives these the biome **grass** colour, a different colormap from foliage. Includes the six terrain grass blocks (argillite, chalk, deepslate, peat, silt, stone), which are surface blocks and so the most visible error. |
| Untinted | 42 | `#ffffff` | Either absent from `RuColors` entirely, or given a fixed / position-dependent colour in code. In game the texture renders as authored; `#ffffff` disables BlueMap's green wash. This is exactly how BlueMap already handles `minecraft:cherry_leaves` and `minecraft:pale_oak_leaves`. |
| Foliage-tinted | 20 | *(none)* | `RuColors` genuinely applies the biome foliage colour, which is already BlueMap's default. Adding entries would be redundant. |
| Placement helpers | 4 | *(none)* | `dirt_placement`, `grass_placement`, `mud_placement`, `sand_placement` -- not real placeable blocks. |

Without the `#ffffff` group, RU's whole visual identity inverts on the map:
wisteria, magnolia, autumnal maple, golden larch, silver birch and the prismoss
family would all render biome-green.

## Known unfixable

`RuColors` computes aspen, enchanted aspen and rainbow eucalyptus from block
position (`Mth.sin` over x/z into `Color.getHSBColor`). No `blockColors.json`
value can express a position-dependent gradient; those render flat.

## Correctness notes

- **No comment keys.** `BlockColorCalculatorFactory.load` switches on the value
  and falls through to `new Color().parse(value)` for anything that is not
  `@foliage` / `@dry_foliage` / `@grass` / `@water` / `@redstone`. A prose value
  throws. Every value here is `@grass` or `#ffffff`.
- **Merging is safe.** Entries are merged with `putIfAbsent` and "higher
  priority resources are loaded first"; the packs folder is read before the mods
  folder and before BlueMap's built-in file. So these win, and only the RU keys
  need listing -- the vanilla mappings still come from BlueMap.
- **Namespace.** `ResourcePack` lists every directory under `assets/` and looks
  for `blockColors.json` in each, so `assets/regions_unexplored/` is read just
  like `assets/minecraft/`.

## Biomes need no fix

RU ships 78 real biome JSONs under `data/regions_unexplored/worldgen/biome/`
with explicit `grass_color`, `foliage_color` and `water_color`. BlueMap 5.7's
`DatapackBiome` parses all three plus `grass_color_modifier`, `temperature` and
`downfall`, and `loadDataPack` feeds it the mod jars whenever
`scan-for-mod-resources` is true. The signed-negative ints RU writes (e.g.
`-8609196`) are handled by `ColorAdapter`'s `NUMBER` branch. Biome colours are
correct out of the box -- the old issue #156 about data-driven biome tints is
long resolved.

## After installing

`/bluemap purge <map>` and re-render. Tile caches built with the wrong tints do
not update on their own.
