# Changelog

Patch notes for the MNMM Create modpack.

---

## 2.1.0

**NeoForge 21.1.234 -> 21.1.249. You must update your client before you can
join.** No world content is lost and no backup is required, but an out-of-date
client will be rejected at connect.

### Removed

- **BlueMap** and **BlueMap Model Loaders**. The server-side web map is retired.
  Nothing is lost in-world — BlueMap only rendered an external map and placed no
  blocks — so this is not a major bump. Its config tree under `config/bluemap/`
  is kept in the repo rather than pruned, per the standing decision on orphaned
  configs.

### Highlights

- **Create: Storage Terminal** gains a **Crafting Storage Terminal**, plus
  **contraption and train support** for terminals. Terminals are now wrenchable.
- **Create Crafts & Additions** adds a **servo motor** and better Sable support.
- **Sophisticated Backpacks / Core** add **linked backpack storage** — link
  backpacks with an Ender Linker to share one inventory.
- **Create: Central Kitchen** lets Mechanical Arms serve feasts across Cultural,
  Rustic and Festive Delights, and load Hearth and Harvest casks recipe-by-recipe.
- **Create: Integrated Farming** adds a unified Roosting JEI category and extends
  Spout feeding from chickens to ducks, geese and turkeys.
- **Create: Dragons Plus** adds an Automated Coloring JEI category for Mechanical
  Mixer dye-fluid recipes.
- **Ultimine Rewind** now rewinds instantly on `Ctrl+Z` when you have the
  materials, and keeps your last 5 rewinds.
- **Create: Enchantment Industry** leaves preview-alpha for a stable release.
- **Complementary Shaders** (Reimagined and Unbound) move to r5.9.

### Notable fixes

- **Copycats+** reverts a regression that **broke air currents from fans**, and
  fixes multistate material crashes on chunk unload.
- **Create Contraption Terminals** fixes a crash with newer Tom's Simple Storage.
- **Create: LazyTick** fixes Basin machines failing to resume after a full-stack
  output extraction.
- **Create: Blocks & Bogies** fixes lighting and rotation on gearless bogies with
  the flywheel backend enabled.
- **Open Parties and Claims** fixes a crash when item stacks merge across a chunk
  border.
- **Bits 'n' Bobs** stops cogwheel chain models twisting, and fixes creative-tab
  crashes from item suppression.

### Pins

- **Artifacts** stays at 13.2.3 and is now pinned (`pin = true`). Reliquified
  Artifacts 1.0.8 declares `artifacts = "[13.2.3]"` — an exact single-version
  range — so any other build hard-fails it, and 1.0.8 is the newest release.
  Nothing is lost: the Quark crash that 13.2.5 fixes was introduced by 13.2.4,
  so 13.2.3 never had it.
- **Create Aeronautics** is no longer pinned, and moves 1.3.0 -> 1.3.2. Its
  `pin = true` turned out to be a leftover from an unrelated commit rather than a
  deliberate hold — no dependency ever required it, and every dependent accepts
  1.3.2. Both releases are bugfix-only; 1.3.2 also fixes JEI creative-tab
  compatibility, which matters given JEI moved in this same release.

### Client instance

`MNMM Create.zip` has been re-exported. You do not need to re-import it — the
pack syncs itself, and packwiz updates the NeoForge version in place — but if you
are setting up a new machine, three things changed:

- It can now be imported **straight from a URL**. Prism's **Add Instance →
  Import from zip** dialog takes a link, so there is nothing to download first.
  The README has it.
- **Memory is now 8 GB fixed** rather than 8-10 GB. ZGC prefers a heap it never
  has to resize, and the old ceiling bought nothing.
- **The server is pre-added** to the multiplayer list, so there is no address to
  type in.

There is also a **README** now, covering joining, the Java 21 requirement and
what the JVM arguments are for.

### Maintenance

- **Create: Wrencheable Planes** moved from `config/paxi/datapacks/` to `mods/`.
  Upstream repackaged the same 2.0 content as a Modrinth-generated mod jar, and a
  `.jar` in Paxi's datapack folder would not have loaded at all.
- **GUI Retextures** re-pointed to the **Dark Mode** build. Upstream ships plain
  and dark as separate versions of one project, and `packwiz update` had silently
  swapped the pack to the light variant.
- Added `README.md` and this changelog; both are excluded from the pack index.
- Audited every pin and every documented version claim in the metafiles. Fixed
  the Copycats+ Additions note, which after this update read backwards (3.0.8 is
  now the top of its range, not the floor), Trash Cans' raised Core Lib floor,
  and thirteen comments still naming the old NeoForge version.

### Known follow-up

- **Create: Food 2.7.1** moved all cross-mod compat to config. Its shipped
  `createfood-client.toml` / `createfood-server.toml` still hold the old
  generated lists, so newly supported compat content will not appear until those
  are regenerated. Left as-is here to avoid clobbering the deliberate toggles in
  the same files.

<details>
<summary>All 93 updated files</summary>

- All of Create Data -> `aoc_data_1.21.1_v5`
- AllTheLeaks (Memory Leak Fix) -> `alltheleaks-1.1.12+1.21.1-neoforge`
- Amendments -> `amendments-1.21-2.1.9-neoforge`
- Balm -> `balm-neoforge-1.21.1-21.0.65`
- CC: Tweaked -> `cc-tweaked-1.21.1-forge-1.120.2`
- Chat Heads -> `chat_heads-0.15.7-neoforge-1.21`
- Climbable Ropes for Create Aeronautics -> `climbable_ropes-2.1.3`
- Complementary Shaders - Reimagined -> `ComplementaryReimagined_r5.9`
- Complementary Shaders - Unbound -> `ComplementaryUnbound_r5.9`
- Crash Assistant -> `CrashAssistant-neoforge-1.20.6-1.21.4-1.11.12`
- Create Aeronautics -> `create-aeronautics-bundled-1.21.1-1.3.2`
- Create Aeronautics x Curios API Compat -> `createaeronauticscurios-neoforge-1.21.1-2.2`
- Create Cobblestone -> `createcobblestone-1.5.0+neoforge-1.21.1-153`
- Create Contraption Terminals -> `createcontraptionterminals-1.21-1.4.0`
- Create Crafts & Additions -> `createaddition-1.7.0`
- Create Encased -> `Create Encased-1.21.1-1.9.0-ht3`
- Create Slice & Dice -> `sliceanddice-4.3.3-neoforge`
- Create Stuff 'N Additions -> `create-stuff-additions1.21.1_v2.1.4b`
- Create: Aeroworks -> `aeroworks-1.5.0`
- Create: Bits 'n' Bobs -> `bits_n_bobs-2.3.0`
- Create: Blocks & Bogies -> `create_bb-1.0.8-1.21.1`
- Create: Central Kitchen -> `create-central-kitchen-2.6.0`
- Create: Connected -> `create_connected-1.3.3-mc1.21.1`
- Create: Copycats+ -> `copycats-3.0.8+mc.1.21.1-neoforge`
- Create: Diesel Generators -> `createdieselgenerators-1.21.1-1.3.15`
- Create: Dragons Plus -> `CreateDragonsPlus-1.11.8b`
- Create: Enchantment Industry -> `create-enchantment-industry-2.5.3b`
- Create: EntityControl -> `CreateEntityControl-0.6.16.10-6.0.x-neoforge-1.21.1`
- Create: Escalated -> `escalated-1.3.2+mc.1.21.1`
- Create: Factory -> `create_factory-0.7b-1.21.1`
- Create: Fantasizing Again -> `create_fantasizing-1.21.1-1.2.0-b3`
- Create: Fast SchematicCannon -> `CreateFastSchematicCannon-2.6.1-neoforge-1.21.1`
- Create: Food -> `createfood-neoforge-1.21.1-2.7.1`
- Create: Hypertubes -> `create_hypertube-0.6.0-NEOFORGE`
- Create: Integrated Farming -> `create-integrated-farming-1.4.1b`
- Create: LazyTick -> `CreateLazyTick-2.6.25-6.0.10-neoforge-1.21.1`
- Create: Power Grid -> `powergrid-mc1.21.1-0.6.1`
- Create: Rubberworks -> `rubberworks-neoforge-1.21.1-1.1.4`
- Create: Rubberworks Compat -> `CreateRubberworks_Compat_v6`
- Create: Schematic Helper -> `createschematichelper-neoforge-mc1.21.1-2.2.0`
- Create: Storage Terminal -> `create_storage_terminal-neoforge-1.21.1-0.1.3`
- Create: Transmission! -> `createtransmission-1.2.2+neoforge-create6-1.21.1`
- Create: Wrencheable Planes -> `create-wrencheable-planes-2.0`
- Cupboard -> `cupboard-1.21.1-4.1`
- Drippy Loading Screen -> `drippyloadingscreen_neoforge_3.1.5_MC_1.21.1`
- e4mc -> `e4mc-neoforge-6.2.1`
- Enchantment Descriptions -> `enchdesc-neoforge-1.21.1-21.1.11`
- Euphoria Patches -> `EuphoriaPatcher-1.10.0-r5.9-neoforge`
- Every Compat (Wood Good) -> `everycomp-1.21-2.11.50-neoforge`
- FancyMenu -> `fancymenu_neoforge_3.9.12_MC_1.21.1`
- Farmer's Delight -> `FarmersDelight-1.21.1-1.3.4`
- Forgified Fabric API -> `forgified-fabric-api-0.116.15+2.3.5+1.21.1`
- FTB Library (NeoForge) -> `ftb-library-neoforge-2101.1.35`
- FTB Quests (NeoForge) -> `ftb-quests-neoforge-2101.1.34`
- FTB Teams (NeoForge) -> `ftb-teams-neoforge-2101.1.11`
- FTB XMod Compat -> `ftb-xmod-compat-neoforge-21.1.11`
- Fusion (Connected Textures) -> `fusion-1.3.15-neoforge-mc1.21.1`
- GUI Retextures -> `GUIRetextures-Dark-2.1`
- ImmediatelyFast -> `ImmediatelyFast-NeoForge-1.6.13+1.21.1`
- Integrated API -> `integrated_api-neoforge-1.21.1-1.8.0`
- Jade Sable Compat -> `sablejade-1.3.0`
- Jade 🔍 -> `Jade-1.21.1-NeoForge-15.10.6`
- Just Enough Breeding (JEBr) -> `justenoughbreeding-neoforge-1.21.1-3.2.1`
- Just Enough Items (JEI) -> `jei-1.21.1-neoforge-19.53.0.425`
- Leaky - Item Lag Fix -> `leaky-1.21-3.4`
- Lithostitched -> `lithostitched-1.8.0+beta4-neoforge-21.1`
- Lootr -> `lootr-neoforge-1.21.1-1.11.38.125`
- ModernFix -> `modernfix-neoforge-5.27.24+mc1.21.1`
- Moonlight Lib -> `moonlight-1.21.1-3.6.1-neoforge`
- Open Parties and Claims -> `open-parties-and-claims-neoforge-1.21.1-0.30.3`
- Puzzles Lib -> `PuzzlesLib-v21.1.56-mc1.21.1-NeoForge`
- Reliable Recipes -> `reliable_recipes-neoforge-1.21.1-3.1.7`
- Reliable Remover -> `reliable_remover-neoforge-1.21.1-2.12.1`
- Sable -> `sable-neoforge-1.21.1-2.0.5`
- Sinytra Connector -> `connector-2.0.0-beta.17+1.21.1-full`
- Sodium -> `sodium-neoforge-0.8.13+mc1.21.1`
- Sophisticated Backpacks -> `sophisticatedbackpacks-1.21.1-3.26.0.2116`
- Sophisticated Core -> `sophisticatedcore-1.21.1-1.5.0.2322`
- Sophisticated Storage -> `sophisticatedstorage-1.21.1-1.5.91.2127`
- Strut Your Stuff (Struts) -> `struts-1.3.1`
- SuperMartijn642's Core Lib -> `supermartijn642corelib-1.1.24-neoforge-mc1.21`
- Supplementaries -> `supplementaries-1.21.1-3.9.7-neoforge`
- Tom's Simple Storage Mod -> `toms_storage-1.21-2.4.2`
- TorchMaster -> `torchmaster-neoforge-1.21.1-21.1.11`
- Trash Cans -> `trashcans-1.1.0-neoforge-mc1.21`
- Ultimine Rewind -> `ultimine_rewind-2.1.0`
- Visual Workbench -> `VisualWorkbench-v21.1.2-1.21.1-NeoForge`
- Waystones -> `waystones-neoforge-1.21.1-21.1.42`
- Waystones: Sable (Create Aeronautics Addon) -> `waystonessable-1.0.7`
- Xaero's World Map -> `xaeroworldmap-neoforge-1.21.1-1.45.0`
- YUNG's API (NeoForge) [1.20.4-1.21.1 ONLY] -> `YungsApi-1.21.1-NeoForge-5.1.8`
- [EMF] Entity Model Features [Fabric & Forge] -> `entity_model_features-3.3.5-1.21-neoforge`
- [ETF] Entity Texture Features - [Fabric & Forge] -> `entity_texture_features-7.2.1-1.21-neoforge`

</details>

---

## 2.0.0

**Back up your world before loading this one.** Two changes in this release
delete blocks that already exist in a save. Everything else is additive, and no
client rebuild is required beyond the usual re-sync.

### Removed

- **Dye Depot**. Every Dye Depot block already placed is gone on load. It was
  dropped for what it did to the rest of the pack, not for its own content: it
  appended its 16 colours to Minecraft's built-in dye list, so every other mod
  that builds blocks by walking that list silently gained 16 variants with no
  models. A test client went from 78 missing models to 750, across 20 mods, and
  the usual companion fix only clawed it back to 572.

### Highlights

- **Dyenamics** and **Dyenamics and Friends** replace it, with 18 new dye
  colours and 1953 coloured variants of other mods' blocks. Both register
  everything in their own namespace, so nothing else in the pack is disturbed.
  Create coverage is better than the old route ever managed: coloured seats,
  valve handles and windmill sails are picked up as genuine Create blocks. The
  trade is that vanilla and Create dyeable blocks do not accept the new colours
  directly.
- **Create: Bits 'n' Bobs** jumps 0.0.44 -> 2.2.9, adding 29 blocks. **This is
  the other world-breaking change:** the small and large cogwheel chains and
  their flanged variants no longer exist as separate blocks. They were
  consolidated into a single cogwheel chain carriage plus a chain pulley, so any
  chain placed under the old version is lost on load. Rebuild them after
  updating.
- **Create: Additional Logistics** adds roughly 160 blocks — flexible shafts in
  every dye and encasing combination, lazy cogwheels and shafts, seats, a
  package accelerator, a package editor, a network monitor and a cash register —
  plus regex support in Create's logistics filters.
- **JourneyPAC** draws Open Parties and Claims claim boundaries on JourneyMap.
  Claims had only ever been visible in Xaero's maps; JourneyMap showed none at
  all.
- A small datapack adds a shapeless recipe turning an inverted redstone link
  back into a plain Create redstone link. The Lights & Controls recipe was
  one-way and did not return the torch.

### Maintenance

- **Azimuth API** and **Strut Your Stuff** ship as new hard dependencies of
  Bits 'n' Bobs 2.2.9.
- Create: Additional Logistics is the third mod holding the pack below Create
  6.1.0, alongside Create: Quarry and Create: Storage Terminal.
- Bits 'n' Bobs' mixin footprint grew from 10+5 to 39+21 and now reaches deep
  into Create's internals. If Create starts behaving strangely, suspect it
  first.

---

## 1.0.0

Retroactive entry. `pack.toml` read `1.0.0` from the first commit on 2026-07-26
straight through to 2026-09-04, so this single version covers roughly six weeks
in which the pack diverged substantially from the CurseForge "All of Create -
Aeronautics" v2.1 it started as. Nothing here needs action now — it is all long
since deployed — so it is grouped by date rather than by consequence.

### 2026-07-26 – 07-30 — Forking from All of Create

- **Storage and building.** **Sophisticated Backpacks** and **Sophisticated
  Storage**, both with their Create integrations, plus **Building Gadgets**,
  **Effortless Building**, **Rechiseled** and **Rechiseled: Create**.
  **Backpacked** was dropped in the same pass, along with 14 unofficial zh-HK
  translation packs and the BisectHosting server menu.
- **Getting around.** **Waystones** with its Create Aeronautics addon, plus
  **Explorer's Compass**, **Nature's Compass** and **Ping Wheel**.
- **Computers.** **CC: Tweaked** and **CC:C Bridge**, wiring ComputerCraft
  computers into Create machinery.
- **Worldgen and decoration.** **Regions Unexplored** and **Tectonic** reshape
  terrain — new chunks only, so they had to land before anyone generated a
  world. The full **Macaw's** set (11 mods: bridges, doors, fences, furniture,
  lights, paintings, paths, roofs, stairs, trapdoors, windows) plus **Every
  Compat** to fill in wood variants.
- **Create: Power Loader**, because the pack had no chunk loader at all and
  factories stopped the moment nobody was online.
- **Fixed a server crash on right-click.** The Carry On Aeronautics compat mod
  had been folded into Carry On itself, and the orphaned patch crashed the
  server the first time anyone interacted with a block. Removed.
- **Menu branding stripped.** The upstream pack's custom main menu art,
  loading-screen customisation and hosting-sponsor menu were all removed.
- **A pack-wide audit.** Six mods were being served to the wrong side — most
  visibly, singleplayer worlds were silently missing several mods, and Tectonic
  and Regions Unexplored could crash a client install. 160 mods were re-pointed
  from CurseForge to Modrinth, byte-identically, so a Prism or MultiMC install
  no longer needs a CurseForge API key. Datapack and shaderpack entries that the
  CurseForge importer had dumped at the pack root, where nothing loads them,
  were moved into place, and the previous author's logs, schematics and 100
  config backups were cleaned out. The file index dropped from 1955 to 598.
- **Chunky** (server-side) for pregenerating the spawn area.

### 2026-07-31 – 08-05 — Recipe viewer, vein mining and terminals

- **The recipe viewer briefly became EMI and then went back to JEI.** EMI could
  not fill recipes at a crafting table in this pack — Visual Workbench replaces
  the table's menu and only ships a JEI transfer handler — so the whole swap was
  reverted inside a day. Net effect for players: none. **JEI** ended up pinned
  to 19.39.0.372, the newest build that runs on the pack's NeoForge, and
  installed **on the server as well as the client**, which is what makes the "+"
  recipe-transfer button actually move items.
- **Just Enough Crafting Tree** kept the one EMI feature worth keeping: full
  recipe trees, including every Create and Create-addon recipe.
- **FTB Ultimine** with **Ultimine Rewind**, **Ultimine Unchained** and **Create
  Ultimine** — vein mining bound to the grave key, with Ctrl+Z to undo the last
  operation, no hardness filtering, and support for Create wrench and material
  interactions.
- **Storage terminals.** **Tom's Simple Storage**, **Create: Storage Terminal**
  and **Create Contraption Terminals**, for reaching a contraption's inventory
  from a single screen.
- **Dynamic lights.** **Create: Dynamic Lights** with **Sodium Dynamic Lights**
  as its backend — held torches and lit contraptions cast real light.
- **Quality of life.** **Comforts** (sleeping bags and hammocks), **Trash
  Cans**, **Create Deco**, **Freecam**, **Light Level Overlay**, **BetterF3**,
  **Just Zoom** (hotkey zoom with adjustable factor) and **Easy Villagers**
  (pick up, trade with and breed villagers as items). **spark** and **Default
  Options** round it out.
- **CC: Sable** connects ComputerCraft to the airship physics backend, exposing
  sub-level and aerodynamics APIs and mass/friction/buoyancy data — flight
  control and autopilot scripting, which nothing in the pack could do before.
- **Create Cobblestone**, a single stress-powered cobblestone generator block
  that does not eat contraption frame space on a fluid setup.
- **Item bans corrected.** Two blacklist entries had typos and had never
  matched: Crushed Magma was disabled but still shown in the recipe viewer, and
  the Gatling Breaker was hidden while remaining fully usable for anyone who had
  one. Both fixed. The **Aeroworks Gyroscope was unbanned** by request — it was
  an upstream ban with no cause recorded here.
- The game window now reads **"MNMM Create"** rather than "Minecraft 1.21.1".

### 2026-08-21 – 09-04 — Build-out

- **Create: Schematic Helper** for uploading and downloading schematics directly
  from createmod.com.
- **BlueMap** served a browser-viewable map of the generated world, with a
  hand-authored colour table so Regions Unexplored's wisteria, magnolia,
  autumnal maple, golden larch and prismoss stopped rendering biome-green. (It
  has since been retired — see 2.1.0.)
- **Create: Quarry**, a kinetic quarry that scans a stake-defined box for ores
  and pushes drops into Create inventories and tanks. The area limit was raised
  from stock to allow a 6x6 chunk pit.
- **Create: Movable Tracks**, **Redstone Pen** (draw redstone wire on surfaces
  instead of placing dust), **TorchMaster** (a mob-spawn-suppressing lantern),
  **Functional Storage** (drawer-style bulk storage) and **Create: Copycats+
  Additions** (corner slopes and slope layers for copycat blocks).
- **Carry On no longer picks up placed Sophisticated Backpacks.** The mod ships
  a blacklist for exactly this, but it used a directory layout that 1.21
  retired, so the file had never loaded and the pack shipped no Carry On config
  to override it.
- **Loot, treasure and maps** landed together at the end of the run:
  **Artifacts** and **Reliquified Artifacts**, **Relics** (Curios-slot equipment
  that levels up), **Lootr** (per-player loot chests, so nobody arrives at an
  emptied dungeon), and **JourneyMap** with **JourneyMap Teams** and
  **JourneyMap Web Map**.
- **Three lighting mods** — **Create: Lights & Controls** (Redstone
  Link-connected lamps, caged signal lights, an Industrial Gear Switch and a
  four-channel Quad Lever Panel), **Cyberish Lights** and **More Lights**.
- **NeoForge 21.1.233 -> 21.1.234** in the same release, forced by Lights &
  Controls, whose only 1.21.1 build refuses to load below .234.
