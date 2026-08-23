# BlueMap config (VPS-managed)

This whole tree is **git-tracked but packwizignored** (`/config/bluemap/` in
`.packwizignore`). Raw non-metafile files in the pack carry no `side` field, so
anything indexed under `config/` reaches every client install -- and BlueMap is
`side = "server"`. Keeping it out of the index means these files are versioned
and reviewable here, but reach neither clients nor the server automatically.

**Deploy by hand**, into
`${CONFIG_PATH}/minecraft/MNMM-Create/config/bluemap/` on the VPS.

No `.conf` files are committed yet -- BlueMap writes its defaults on first boot
and the tuned ones can be committed back afterwards if that history is wanted.
This file is the reference for what to set in the meantime. `packs/` holds the
hand-authored `regions-unexplored-blockcolors/` pack; the two downloaded
artifacts stay manifest-only (see `packs/README.md`) because this repo contains
no jars.

## Tuning

Chosen for this box: 8 cores at ~1.20 load, 12 GB heap, 2 players, a
few-thousand-block pregen.

### `core.conf`

Write this one by hand on the VPS **before** the first boot -- see Rollout
below. `accept-download` in particular is not optional.

| Key | Value | Why |
|---|---|---|
| `accept-download` | `true` | Mandatory. Downloads a Mojang client jar (~25 MB) for vanilla resources; BlueMap will not start without it. |
| `data` | `bluemap` | Resolves to `/data/bluemap` in the container. |
| `render-thread-count` | `2` | BlueMap runs **inside the server JVM**, sharing the 12 GB heap and competing with the main tick thread. 2 leaves headroom at 1.20 load. Negative values mean cores-minus-n (`-6` is equivalent); prefer the explicit number. Raise to 4 for the initial backfill if impatient, then put it back. |
| `scan-for-mod-resources` | `true` | **Essential** -- this is the setting that makes Create render at all. |

### `webserver.conf`
`enabled: true`, `port: 8100`. Leave `ip` alone. It is not in the generated
template but defaults to `0.0.0.0` in `WebserverConfig.java`, which is what the
reverse proxy needs in order to reach the container.

### `webapp.conf`
Defaults are fine. `default-to-flat-view: false` keeps the perspective view,
which reads terrain better. If the map feels heavy in the browser, the knobs are
`resolution-default`, `hires-slider-default` (100) and `lowres-slider-default`
(2000).

### `plugin.conf`
| Key | Value | Why |
|---|---|---|
| `live-player-markers` | `true` | The main thing running BlueMap as a mod buys over the standalone CLI. |
| `player-render-limit` | `-1` | Do **not** set this to 1 or 2. With two players any positive value would pause rendering essentially permanently. |
| `full-update-interval` | `1440` | Default. A consistency sweep, not a re-render. |

### `maps/*.conf`
One per dimension, generated after the first boot.

| Key | Value | Why |
|---|---|---|
| `min-inhabited-time` | `0` | **The most important setting here.** Anything above 0 hides pregenerated chunks nobody has visited -- exactly the chunks this map exists to scout. |
| `remove-caves-below-y` | `55` | Default. Large render-time and storage win. |
| `max-y` (nether) | auto ~`90` | Cuts the nether ceiling; BlueMap sets this itself. |

If disk gets tight, set `min-x` / `max-x` / `min-z` / `max-z` bounds in the
overworld config **before** the first render rather than purging afterwards.

## Reverse proxy — Pangolin

The webserver is deliberately not published on the host. `compose.yaml` attaches
the container to an external network named by `${PROXY_NETWORK}` in `.env`;
point the Pangolin resource at `http://MNMM-Create:8100` (the container's
`hostname`).

Live player data is **Server-Sent Events** on `/maps/<id>/live/players`. The
proxy resource must not buffer responses and needs a generous read timeout, or
the player markers stall while the rest of the map works fine — which makes this
an easy thing to misdiagnose later.


## Rollout — seed before the first boot

Order matters. Do **not** let BlueMap boot on generated defaults first:

- `accept-download` defaults to `false` and `BlueMapService` throws
  `MissingResourcesException` before loading resources, so a default first boot
  renders nothing anyway.
- Installing `packs/` afterwards means `/bluemap purge` and rendering the entire
  world a second time.

1. On the VPS, create
   `/opt/Docker/container-data/minecraft/MNMM-Create/config/bluemap/` and write
   `core.conf` there with the values from the Tuning section above. At minimum
   it needs `accept-download: true`, or `BlueMapService` throws
   `MissingResourcesException` and nothing renders.
2. Create `packs/` under it and install both artifacts per `packs/README.md`
   (verify the sha256 sums).
3. **Do not create a `maps/` directory.** BlueMap populates map configs only
   when the folder is absent (`BlueMapConfigManager`:
   `if (!Files.exists(mapConfigFolder))`). An empty `maps/` means zero maps and
   nothing renders.
4. Add `PROXY_NETWORK=<pangolin network>` to `.env`.
5. Push and redeploy. BlueMap generates `webserver.conf`, `webapp.conf`,
   `plugin.conf` and `maps/*.conf` with defaults, loads the packs, and renders
   once, correctly. No purge needed.
6. Add the Pangolin resource pointing at `http://MNMM-Create:8100`.
7. Commit the generated configs back for the record.

Watch the first render with `/bluemap status`. Region data is ~1.8 GB, so this
is a moderate world, not a marathon.

## After changing resource packs

`/bluemap purge <map>`, then let it re-render. Cached tiles built against a
different pack produce scrambled textures.
