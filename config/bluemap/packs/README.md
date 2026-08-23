# BlueMap addon packs

BlueMap loads every `.jar` and resource-pack `.zip` in this directory
(`BlueMapConfigManager`: `packsFolder = configRoot/packs`; on NeoForge
`configRoot` is `config/bluemap`, from `ForgeMod.getConfigFolder()`).

These artifacts are **not committed** -- this repo contains no jars. Install them
on the VPS with the commands below. The hashes are the authority; verify before
restarting the server.

## Install

Run inside `${CONFIG_PATH}/minecraft/MNMM-Create/config/bluemap/packs/`:

```sh
curl -LO https://github.com/BeneHenke/BluemapCreateEntityAddon/releases/download/v1.2.1/createentityaddon-1.2.1-5.7.jar
curl -LO https://github.com/Uiniel/BlueMap-Create-Resource-Pack/releases/download/v1/BlueMap-Create-Resource-Pack.zip

sha256sum -c <<'SUMS'
be27b73e8ff8667f007a26e5d6328ec0bd1cf0c39898c60c07e6c1c208a0bea8  createentityaddon-1.2.1-5.7.jar
d022abaeac28e6f4edb200e6d928d117ad3a0367a9383a342327cc7fa52da669  BlueMap-Create-Resource-Pack.zip
SUMS
```

Then restart the server and `/bluemap purge <map>` -- adding a resource pack
invalidates every cached tile, and skipping the purge gives scrambled textures.

## What each one is

### `createentityaddon-1.2.1-5.7.jar`
[BeneHenke/BluemapCreateEntityAddon](https://github.com/BeneHenke/BluemapCreateEntityAddon).
Renders Create **contraptions and train tracks**, which BlueMap cannot do on its
own -- contraptions are entities, not blocks, and BlueMap renders the world.

**Take the `-5.7` asset specifically.** The v1.2.1 release also ships `-5.12`,
`-5.13+` and `-5.22+` variants that will not load against BlueMap 5.7. Verified
match: its `build.gradle` declares `compileOnly("de.bluecolored:bluemap-core:5.7")`,
`bluemap-common:5.7`, `bluemap-api:2.7.4` -- exactly what BlueMap 5.7 ships --
and `minecraft_version_range=[1.21.1]`.

Upstream notes copycats still have issues.

### `BlueMap-Create-Resource-Pack.zip`
[Uiniel/BlueMap-Create-Resource-Pack](https://github.com/Uiniel/BlueMap-Create-Resource-Pack) v1.
Pure data, no code: 59 blockstate files overriding `assets/create/blockstates/*`
plus 40 static models under a `bluemap_create` namespace. Create assembles many
of its models in the block-entity renderer at runtime, where BlueMap's parser
cannot see them; this pack substitutes static equivalents.

Covers 28 block families. Working here: gearboxes, mechanical drill/press/saw/
pump, millstone, deployer, clutch, gearshift, encased chain drive, encased fan,
fluid pipes, hand crank, postbox, cuckoo clock, speedometer/stressometer,
portable storage interface, weighted ejector, mechanical/windmill/clockwork
bearings, adjustable chain gearshift.

**Not working here:** the blocks upstream marks `(*)` -- belts, water wheels,
mechanical mixer, mechanical arm, bogeys, blaze burner, fluid valve. They need
BlueMapModelLoaders >= 0.4.2, which needs BlueMap >= 5.14, which needs a newer
MC than this pack runs. See `mods/bluemap-model-loaders.pw.toml`.

Upstream also lists as impossible-by-resource-pack regardless of version:
andesite casing on belts (reads as brass), water wheel wood types, wooden and
metal brackets, connected fluid tank textures, fluid pipe rim detection.

## Also here

### `regions-unexplored-blockcolors/`
Hand-authored in this repo, not a download. Regions Unexplored registers block
tints in code, which BlueMap cannot read, so 82 RU blocks would render with
biome foliage green. See that folder's README. Copy the folder across with the
two downloads above.

## Not installed

`create-bluemap` (Szedann) adds live train position markers and schedules, and
is also built against BlueMap 5.7 exactly (`bluemap_version = 5.7`,
`runtimeOnly("maven.modrinth:bluemap:5.7-neoforge")`). Deliberately skipped on
the first pass -- the Entity Addon already draws track geometry. If added later
it goes in `mods/` as a normal Modrinth mod (`side = "server"`), not here.
