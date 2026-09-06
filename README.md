# MNMM Create

A NeoForge 1.21.1 Create modpack built around **Create: Aeronautics** — airships,
contraptions and large-scale factory automation. Run for a small group of friends.

**Minecraft** 1.21.1 · **NeoForge** 21.1.249 · **289 mods**

**[Read the changelog →](CHANGELOG.md)** — patch notes for every version: what you
gain, what breaks, and whether you have to update your client before you can join.

## Joining

### 1. Import the instance

Prism Launcher → **Add Instance → Import from zip**. That dialog takes a
link as well as a local file, so you need not download anything first — paste:

```
https://raw.githubusercontent.com/dKatsuro/MNMM-Create/main/MNMM%20Create.zip
```

It must be that `raw.githubusercontent.com` link. The `github.com/...blob/...`
page URL serves HTML, not the zip, and the import fails on it.

The zip contains no mods. It is only the instance shell — Minecraft 1.21.1,
NeoForge, the JVM arguments, and a pre-launch command that runs
`packwiz-installer` against this repo. Mods, configs, resource packs and shaders
are downloaded on first launch and re-synced on every launch after that.

### 2. Point Prism at a Java 21 runtime

The instance deliberately ships with **no Java path of its own** — it uses
whatever Java is set globally in Prism. If that is unset, launching fails with
*"Pre-Launch command failed with code 0"*. Code 0 means the process never
started: the pre-launch command is `"$INST_JAVA" -jar packwiz-installer-…`, and
with no Java there is nothing for Prism to run.

To set it for this instance only:

1. Select the instance → **Edit**
2. **Settings** in the sidebar → the **Java** tab
3. Tick the **Java installation** box, so the instance overrides the global setting
4. Click **Detect** and choose a **Java 21** entry

If **Detect** turns up nothing, click **Open Java Downloader** and take
**Mojang → 21.0.7**. That is `java-runtime-delta`, the runtime Mojang ships for
1.21+, and it is what this pack is tested on.

**It has to be Java 21.** Minecraft 1.21.1 and NeoForge 21.1.x target Java 21;
anything older will not launch, and anything newer breaks one of the pack's GC
flags — see [Java notes](#java-notes).

### 3. Check the memory allocation

The instance asks for **8 GB, fixed** — minimum and maximum are both 8 GB. On a
machine with much under 16 GB of RAM that fails at launch, because the JVM
cannot reserve a heap that large alongside Windows and everything else running.

Lower it in **Edit → Settings → Memory**. 6 GB for both is a workable
floor; below that, expect stutter around large contraptions and long
chunk-loading pauses on an airship.

The **PermGen** field shows 128 MB. Ignore it — PermGen was removed from the
JVM in Java 8, so on Java 21 the setting does nothing. It is a leftover of the
Prism config format, not a tuning knob.

### 4. Launch

First launch downloads the full mod list, so it takes a few minutes with no
visible progress beyond the Prism console. Later launches only fetch what
changed.

## Staying up to date

The pack syncs itself from this repo every launch, so you never download a mod by
hand and you never need to re-import the zip.

**That includes the NeoForge version.** packwiz-installer reads Prism's
`mmc-pack.json` — the instance's own loader metadata, one folder above
`minecraft/` — compares its `net.neoforged` component against `[versions]` in
`pack.toml`, and rewrites it. No extra flags are needed: the pre-launch command
already does this, because the installer's `--multimc-folder` defaults to `..`,
which lands exactly on the instance root.

What you actually see is a **confirmation dialog** listing the old and new
versions. Two things worth knowing about it:

- **Accept it.** If you decline, the installer logs *"Update cancelled by user!
  Continuing to start game…"* and launches on the old loader, and the server will
  refuse the connection. Quit and relaunch to get the prompt back.
- Prism re-resolves the loader on that same launch, so there is no second restart.

So when the changelog says a release changed NeoForge and you must update before
you can join, the whole procedure is: launch once, click through the prompt.

Automatic NeoForge handling needs packwiz-installer **0.5.14 or newer**. The zip
ships only the bootstrap jar, which pulls the current installer itself, so this
sorts itself out.

## Java notes

The instance overrides Prism's JVM arguments with:

```
-XX:+UseZGC -XX:+ZGenerational -XX:+DisableExplicitGC -XX:+PerfDisableSharedMem -XX:+UseDynamicNumberOfGCThreads
```

That is generational ZGC in place of the default G1 — it gives up some raw
throughput in exchange for sub-millisecond pauses, which is the right trade for a
Create pack where what you actually notice is a frame hitch mid-contraption, not
a slightly lower average tick rate. `DisableExplicitGC` stops mods calling
`System.gc()` and forcing a collection nothing asked for.

**Stay on Java 21, and specifically on a 21.x build.** There is nothing above it
worth chasing:

- `-XX:+ZGenerational` was introduced in Java 21 as the opt-in for generational
  ZGC. Java 23 deprecated it; Java 24 removed non-generational ZGC entirely
  ([JEP 490](https://openjdk.org/jeps/490)) and made the flag obsolete, so the
  JVM warns and ignores it. A later release drops recognition of it altogether,
  and at that point the JVM refuses to start rather than warn.
- Above 21 the flag buys nothing anyway — generational ZGC is the default there.
- Java 21 is also the version NeoForge 21.1.x and Minecraft 1.21.1 are built
  against. Newer JDKs are untested territory for the mod list, not free
  performance.

**Which** Java 21 build matters far less than people claim. Mojang's 21.0.7 is
the zero-effort choice and the one this pack runs on; Temurin 21, Zulu 21 and
Microsoft OpenJDK 21 are the same OpenJDK underneath and perform identically
here. GraalVM's JIT gets recommended for Minecraft now and then, but the gains
are inconsistent and it is not worth the setup for this pack.

`Xms` and `Xmx` are both set to 8 GB deliberately, rather than a range. ZGC prefers
them equal — a fixed heap means it never pauses to resize mid-session — so if you
do raise the ceiling for a bigger base, raise the floor with it and keep the two
matched.