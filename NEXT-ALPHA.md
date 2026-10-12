# What comes next

*Oxygen Not Included: now with actual reasons for the oxygen to be not included.*

This page says what Alpha 2 of the Oxygen Not Included simulation SDK is, what it does not do yet,
what is being built for later releases, and how releases work. Everything below the Alpha 2
sections is a plan, not a promise: scope moves to fit the release date, and anything not ready
waits for the following release.

## Alpha 3 (in development)

Alpha 3 is the next release. What it carries is decided at its cut-off (see
[How releases work](#how-releases-work)); everything in this section is a plan, not a promise, and
anything not ready moves to a later release.

**Built, in the preview videos** (see [Preview](#preview)); candidates, not commitments:

- Planetary Logistics (Mod 4): a first trade route between planets.
- A chlor-alkali cell, the first of the chemistry work.
- Navigator, an in-game reference for the elements and substances.
- Building art that reloads into a running game, planned for its own repository, `oni-livedev`;
  a first step towards an in-game kanim editor (see [Preview](#preview)).

**In development:**

- Multiplayer: lockstep co-op, with prediction and rollback.
- Performance: more of the game's own per-frame code, continuing the performance mod released
  alongside Alpha 2.

**Scoped, not started:**

- The part of the API that runs on the game's own simulation, `OniFramework.Library`, moves to its
  own repository.
- An audit of every place the simulation library empties or rewrites a cell, following the dig fix
  in Alpha 1.1.
- Two further simulation speed changes (see [What is being worked on](#what-is-being-worked-on)).

## Alpha 2

Alpha 2 adds to everything in Alpha 1; no Alpha 1 interface is removed or changed.

| repository | in Alpha 2 |
|---|---|
| [oni-sim-replacement](https://github.com/Salacious-Oni-Dev/oni-sim-replacement) | library version 0.2: liquid depth pressure, several diseases per cell, an odour field, airborne particulate (smoke), condensation, heat exchange and stirring in gas-mixture rooms, an ignition event stream, two new save-format versions, and the dig fix from Alpha 1.1 |
| [oni-framework-api](https://github.com/Salacious-Oni-Dev/oni-framework-api) | API level 0.2: combustion and gas fires with their visuals, particulate, multi-disease and odour, hydrostatics, falling-liquid heat exchange, custom room types, building claims, reversible pipe ports, pipe flow direction, a determinism digest, and more |
| [oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods) | Mod 1 gains the Active Vent, the Steam Room, germs that travel through pipes, and flow arrows; Mod 2 gains fire, the Campfire, Sparker, Fire Sprinkler and Mash Tun, Soot, hygiene, and a rebuilt Hydrogen Generator; and a new mod, Mod 5 (Automation / IC10): Stationeers' IC10 programmable chips reading and driving the game's own buildings, with AI placeholder art |
| [oni-sim-visualizer](https://github.com/Salacious-Oni-Dev/oni-sim-visualizer) | a standalone viewer for the simulation's state, from a recording or a running game |
| [oni-dev-environment](https://github.com/Salacious-Oni-Dev/oni-dev-environment) | a debuggable development copy of the game |
| [oni-sdk-docs](https://github.com/Salacious-Oni-Dev/oni-sdk-docs) | the guides that cover the SDK as a whole, this page, and [Art wanted](ART-WANTED.md) |

Each repository's `CHANGELOG.md` lists the release in full.

**Alongside the SDK.** Two performance mods are released at the same time under
[Salacious-Oni-Mods](https://github.com/Salacious-Oni-Mods). They do not need the SDK, and the
SDK does not need them:

- **oni-perf-mod:** frame-time fixes to the game's own code, and faster loading of large colonies.
  Its fast loader ships switched off; see its README for what it does, and for the three files
  that are not under the MIT licence.
- **oni-gc-runtime:** a garbage-collection timing mod and a rebuilt runtime that shortens the
  pause of a collection.

### Mod 5, Automation / IC10

New in Alpha 2, the first release of the third flagship: Stationeers' IC10 language in the game,
all 154 instructions, written and edited in an in-game editor, with chips and logic units that
read and drive the game's own buildings over a data cable. See its
[README](https://github.com/Salacious-Oni-Dev/oni-flagship-mods/tree/main/mod5-automation-ic10).
Its interpreter follows Stationeers' own closely and is not under the MIT licence; it will be
rewritten later (see that repository's NOTICE). Its building art is AI placeholder art.

**`ic10-in-game.mp4`** (3:29), attached to the
[Alpha 2 release](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/releases/tag/v0.1.0-alpha.2):
a test world of the automation buildings, checked live: the chips run their programs against the
buildings on their data network, and the editor and device windows are opened and checked. One
known issue shows in the run, in how a solar panel's maximum output is read (see Known
limitations).

### Also in Alpha 2

- **Bottle and canister emptiers start at once.** An emptier that is placed or loaded already
  holding something starts emptying it, instead of waiting for another delivery. This changes
  the game's own behaviour for every game with the framework.
- **Fixed:** every liquid pipe line read "Critical Vaporization" in its tooltip.
- The smoke system came from an idea by Kaz, in the Oxygen Not Included community Discord.

## Known limitations of Alpha 2

Alpha 2 is an alpha. These are the things it does not do yet, or does only roughly, so that
testers know what to expect.

**Platform and installation**

- **Windows only.** The simulation library is a Windows x64 DLL, so the SDK needs the game's
  Windows version. On any other platform the framework leaves the game's own library in use, and
  Mods 1 and 2 cannot do their work.
- **One game build.** Alpha 2 supports game build 744825 only. On any other build the framework
  and the installer leave the game's own library in place.
- **A game update puts the game's own library back.** Install again only with a release that
  lists the new build. See
  [After a game update](guides/installing.md#after-a-game-update).
- **The first acceptance restarts the game once** when Mod 1 is enabled, because the game has
  already loaded its own library by the time the consent dialog appears.
- **Build version, not release number.** The watermark, `FrameworkVersion` and `SimVersion` show
  a build version; the release is the tag. See [Compatibility](guides/compatibility.md).

**Saves**

- **A world in which a mod stored extension data is saved in a newer format,** which the game's
  own library is not built to read. Alpha 2 adds two newer versions, for cells with several
  diseases and for cells holding carried heat. Keep a copy of saves before removing the SDK, and
  keep the framework enabled for those saves.

**Simulation and gameplay**

- **Several diseases per cell is off unless a mod switches it on,** and no flagship mod does yet.
  With it off, two diseases that meet still destroy one another, as in the game itself.
- **Liquid depth pressure is off by default.** Mod 1 switches it on only when the game is started
  with `--mod1-hydrostatics`. Sealed tanks and a dedicated API for siphons are later stages.
- **The odour field has little gameplay yet.** Grime in Mod 2 smells; nothing else in the game
  emits odour.
- **Stand-in art.** The Active Vent, Sparker, Fire Sprinkler and Mash Tun wear a vanilla building's
  animation until their own art is made ([Art wanted](ART-WANTED.md)). Mod 5's buildings wear AI
  placeholder art, to be replaced.
- **Balance is not final.** Mod 1 and Mod 2 are demonstrations of what the simulation can do. Their
  numbers, and how hard they make a colony, can change between releases.
- **No planet or logistics yet.** Those are the second flagship ([Preview](#preview) and
  [Further out](#further-out) below), and neither is in Alpha 2. Programmable automation, the
  third flagship, is new in Alpha 2 as Mod 5.
- **Mod 5 and the Solar Panel.** With Mod 2 installed, a chip reads a Solar Panel's `Maximum` as
  half of the panel's real rating. A fix is planned.
- **Mod 5 drives only a building's first automation input,** and the IC Memory and IC Sorter have
  no player controls of their own yet; a program sets them.

**Compatibility with other mods**

- **Mods that replace the simulation library, or that load it before the framework,** stop the
  framework from providing it; the game's own library is then used for that session. Keep the
  framework at the top of the mod list.
- **Mods that patch the Electrolyzer or the Hydrogen Generator** have those patches set aside
  while Mod 2 is enabled, because Mod 2 rebuilds both buildings. Each removal is written to the
  log.
- **FastTrack,** the widely used community performance mod, has been run alongside the framework
  and Mods 1 and 2 without errors. It replaces the code path Mod 1 uses to draw its overlays,
  though, and whether Mod 1's overlays still update under it has not been checked yet.

**Development tooling**

- **Development only:** the debug servers have no authentication. Start them only on a trusted
  network. See [the full note](guides/simviz.md).

## How releases work

Releases follow a regular cadence, currently a trial of about ten days between releases. Each
release ships what has passed its checks by its cut-off; anything late moves to the next one.
**The scope moves, not the cut-off:** nothing holds a release back for a feature that is late.
The interval itself is still on trial, so it is not a promise.

Every release has one version number (`0.1.0-alpha.1`, `0.1.0-alpha.2`, and so on), tagged in
every repository. The [Compatibility](guides/compatibility.md) guide covers numbering, supported
game builds and what a game update means. Each repository's `CHANGELOG.md` records what changed.

### Cut-offs

- **Scope freeze.** A few days before each release, scope is frozen. Only work that is in the
  development repositories by then can ship; nothing is added to a release after its freeze.
- **After the freeze, only fixes.** Between the freeze and the release, the only work done for
  it is fixing problems that would stop it shipping.
- **New work starts early.** New features start only in the first part of a cycle. Anything
  proposed later is planned for the release after.

### How a candidate is checked before it ships

A change to the simulation library has to pass, at the freeze:

- **The bit-exact comparison with the game's own library.** Recorded scenarios are run through
  the game's own `SimDLL.dll` and through the replacement, and the output must match exactly.
  This is what makes "behaves like the game until a mod uses its extensions" a checked claim
  rather than a hope. A feature that changes behaviour ships off by default or opt-in, so that
  with it off the comparison still matches. Alpha 2 makes one exception, and names it: liquid
  that the game's library deleted when a cell turned into a falling drop is now kept. The
  `FlowKeepInflow` setting at 0 restores the game's arithmetic (see the
  [library's changelog](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/main/CHANGELOG.md)).
- **The offline suites.** White-box tests of the simulation's kernels, the extension surface and
  the save format, plus fixed-world checks whose cell counts and state digests are recorded and
  must not move unless a change means them to.
- **A performance benchmark.** The simulation's kernels are timed against a recorded baseline,
  on worlds that have been left to settle first, for any change that could affect speed. It is
  not yet an automatic gate.
- **Live runs in the game.** Scripted test scenarios run in a real game with the SDK installed,
  each ending in a pass or fail verdict. The showcase videos are recordings of such runs. A
  failure is investigated before the release, and is either fixed or listed as a known issue.
- **An install test.** A fresh copy of each repository is built from source, installed, played
  and removed again, and the game's own library must be back in place afterwards.
- **A wording and content check** on everything that goes into a public repository.

These checks run in the development repositories before every release. The public repositories
have no automated checks of their own yet; the bit-exact comparison needs the game's own library,
so it could not run on a public build server in any case.

### How fixes are picked

Each reported problem gets one of three labels:

| label | what it means |
|---|---|
| must fix | a crash, a corrupted save, an install or restore that fails, or anything wrong in public text. Worked on at any time, including in the last week |
| known issue | ships, with a line in the release notes |
| next release | does not ship in this release |

### How issues feed in

Issues filed in the public repositories (see "Following along and reporting issues" below) are
read and labelled as above. A bug report with the version, game build, installed `SimDLL.dll`,
enabled mods and `Player.log` can usually be reproduced; one without them often cannot. A fix
suggested in an issue is ported into the development repositories by hand, and reaches the
public ones at the next release.

### Suggesting an idea

Have an idea for the simulation, the API or one of the mods? Open an
[idea request](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/issues/new?template=idea-request.yml)
on this repository. Check the open requests first: someone may already have asked for it. Say what
should happen in the game, why it would make the game better, and which part it touches if you
know. Every request is read. An idea that is taken up goes into planning and is credited by name
in the release notes, unless you would rather not be named.

Ideas are not promises: anything accepted lands in the release whose cut-off it makes. Artists:
[Art wanted](ART-WANTED.md) lists the building animations that are needed.

## Performance

### What the SDK already gains

None of these numbers compares Alpha 2 with Alpha 1, and none compares the whole SDK with the
unmodified game. Most are a before-and-after of one change, made before Alpha 1 and still in
Alpha 2: the same code just before the change against just after it, on the same test world and
the same development machine; the rest give what one part of a frame costs. The simulation-library numbers come from the library's own offline benchmark on
test grids, except where a bullet names a test save in the game; where a bullet measures against the
game's own library, it says so. They show the size of each change; they are not a promise about any
particular colony or computer.

**In the simulation library**

- **The simulation frame runs on a worker thread,** as the game's own library does, while the game
  carries on. When this landed, the time the replacement added to each game frame over the game's
  own library fell from 1.73 ms to 0.40 ms on a large test save, and from 2.53 ms to 0.42 ms on a
  mid-sized one. Its output is byte-identical to running the frame on the game's thread. Measured
  later on a large test save with Mods 1 and 2 running, the simulation cost the game's main thread
  about 0.5 to 0.6 ms per simulation tick, 2.7 % of its frame; the rest of the simulation's work
  is hidden on the worker.
- **Compiler optimisation and a baseline instruction set** (`-O3`, and the x86-64-v2 instruction
  set). Against the same library built without them, on a 256 x 384 test asteroid the frame got
  7.3 % faster, and on a settled, mostly-rock test world 22.1 % faster, with identical output. A newer instruction set was tried and refused, because it changed results.
- **Each world in a cluster refreshes only its own area.** The game runs the simulation once per
  discovered world, and the replacement used to copy the whole grid for each one. On a test
  cluster cut into twelve worlds, with the same cells to simulate, the frame got 19.6 % faster;
  a single world is unchanged.
- **The simulation stops republishing what has not changed.** On a 512 x 768 test grid, the frame
  went from 5.88 ms to 3.72 ms, and the worst frame from 7.74 ms to 5.36 ms.

**In the framework and Mod 1**

- **Mod 1 stopped producing garbage every frame.** An early version of Mod 1's overlay code
  allocated fresh buffers on every frame. Fixed before Alpha 1, it took the managed heap's growth
  on a large test save from 85 to 116 MB every 10 seconds down to 17 to 25 MB (the game alone
  grows 0.4 to 6 MB), so garbage collections, and the stutter they cause, come much less often.
  On three test scenes the frame rate rose from about 135 to 160 fps to about 245 to 265 fps.
- **An overlay cache is available, off by default.** `OverlayTextureCache` stops four of the
  game's overlay texture passes from recomputing an area that has not changed. Those four cost
  about 0.71 ms a frame on a late-game test colony and produce identical output on 86 to 99 % of
  the frames they run. It changes how the base game draws, so a mod has to switch it on; Mod 1
  installs it and leaves it off.
- **The watermark shows which garbage collector is loaded** (`GC STOCK` for the game's own), so a
  bug report says which one was in use.

### What is being worked on

- **Where a frame actually goes.** On a large test save with the SDK and Mods 1 and 2, about 78 %
  of the main thread's frame is the game's own managed per-frame code, and the simulation is a
  small part of it. The larger gains are in the game's managed code; the performance mod released
  alongside Alpha 2 is the first of that work.
- **Overlay textures.** Extending the overlay cache to more of the game's overlay passes needs
  change information from the simulation, which is being scoped.
- **More simulation speed.** Two further changes are scoped and not built: skipping cells that
  provably cannot change (on a settled test asteroid, heat conduction alone would fall by about
  half), and running the empty-cell pass only where something changed.

## Preview

This is work that is built in development but is **not** in Alpha 2. It is shown so testers can
see where the SDK is going. Any of it can change, slip, or be dropped, and none of it is a promise
of a feature or a date. The videos are recordings of the automated test runs, so the numbers on
screen are read out of the running game. Most of the buildings in them wear stand-in or draft art.

The videos and screenshots are attached to the
[Alpha 2 release](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/releases/tag/v0.1.0-alpha.2).

**`first-trade.mp4`** (5:48): **Planetary Logistics (Mod 4), the first trade.** A rocket flies
1,000 kg of liquid carbon dioxide to a trade depot three rings out and brings water home. The
film follows the outbound flight, the exchange at the depot, the landing and unloading, and the
books: the carbon dioxide and the water are each accounted for across the whole trip. A second
trip is then refused, because the depot's water stock has fallen below one lot, and the rocket
comes home with its cargo. The game's own welcome dialog shows behind the briefing card in the
first seconds.

**`chlor-alkali-cell.mp4`** (2:51): **A chlor-alkali cell.** Electricity splits salt water into
chlorine, hydrogen and caustic soda, in a 3x3 building drawn from a concept sheet. Every kilogram
and every joule is counted against the simulation's own books, and the run checks the reaction's
proportions as it goes. One check in this run does not pass: a small energy remainder is not yet
accounted for, and is being worked on. The building and its new substances belong to the
chemistry work planned for a later release.

**`kanim-hot-reload.mp4`** (0:35): **Building art, reloaded into a running game.** A building's
animation is replaced while the game runs, and the change shows within a second, without a
restart. This is development tooling for making building art faster. It is intended as a first
step towards an in-game kanim editor and creator suite, for building, editing and fully animating
kanim art right in the game.

**Navigator** (`navigator-*.png`): an in-game reference for the expanded simulation. One tab is
the periodic table, showing which of the 118 elements the game has; the other lists every
substance with its properties, its phase diagram, its heating curve, a buoyancy ladder of the
gases, and, for a planet, how a substance behaves across a day.

**Multiplayer:** lockstep co-op with prediction and rollback is in progress. There is no new
footage in this release.

### Coming with the chemistry work

The chlor-alkali cell is the first of a larger chemistry set, built in development and not in
Alpha 2: 31 more real elements as data (among them calcium, vanadium, platinum, sodium, silicon,
gallium, argon, neon, krypton, xenon, titanium and silver), and about two dozen real compounds
with their own phases, such as sulfur dioxide, sulfuric acid, hydrochloric acid, caustic soda,
quicklime and gypsum. The vanilla game has 21 real elements; Mod 2 adds a 22nd, nitrogen.

## Further out

The SDK's long-term plan is five gameplay mods in three flagship releases. Alpha 1 and Alpha 2
carry the first flagship (Mods 1 and 2). The other two are further out, and their order and
timing are not fixed.

As with the first flagship, the aim is that everything these mods do is reachable through the
public API, so other mods can build the same things.

### Flagship 2: Dynamic Planet (Mod 3)

**What it is meant to do.** Turn the colony's home from an isolated asteroid into a planet with an
environment of its own. Mars is the first planet. Its atmosphere, temperature and daylight are
gameplay: heat has somewhere real to go, and the air itself is a resource.

**What already works in development builds.** This runs in development builds and has been
checked by scripted runs in the game:

- **Mars generates as its own world,** and is the starting world of its own cluster. It is reached
  through a new game mode on the game-mode screen, with its own planet screen that lists what the
  colony lands with. It has a surface with hills and a landing plain, and no geysers, ruins,
  wildlife or meteors: nothing on Mars hands over resources for free.
- **The colony lands in a rocket.** It arrives in a landed rocket built from the game's own rocket
  parts, with the crew in Atmo Suits inside its pressurised cabin. The engine is broken and both
  tanks are empty. Leaving needs steel for repairs, plus liquid hydrogen and oxygen, which means
  building the refrigeration cycles the first flagship demonstrates.
- **A real atmosphere.** The surface holds carbon dioxide at Mars' thin pressure, 2.47 kPa. The
  planet refills what a machine takes out of the open sky, and books the energy that brings in.
- **Day and night.** The ambient temperature runs from about 293 K at noon to about 220 K at
  midnight. The planet itself absorbs and radiates heat: sunlight heats what it reaches, the ground
  radiates to the sky, and a colony's waste heat can go into the planet. The swing is also the
  design's capture window. By day the thin air cannot be liquefied, but compressing cold night air
  can produce liquid carbon dioxide.
- **A moving sun.** The sun follows Mars' orbit across a 100-cycle year, so sunrise, sunset and
  the sun's height change through the year. Shadows fall away from the sun rather than straight
  down, and light passing through glass, water or thin gas is dimmed as in the game's own light
  rules.

The simulation side of the planet environment is already in the public library. The API that
drives it, and Mod 3 itself, are not public yet.

**Planned, not built:** machines that extract and process the atmosphere, the liquid carbon
dioxide refrigeration loop, dust, hazards, and more planets after Mars.

### Flagship 2: Planetary Logistics (Mod 4)

**What it is meant to do.** Once there is more than one colony, connect them: landing pads, cargo,
storage, a console for orders and trade, and planets that need to import what they lack. It is
built on the game's own rockets rather than a new transport system.

**Status: built in development.** The first trade route runs: a rocket carries cargo to a trade
depot and brings the goods it bought home, with everything it carries accounted for (see
[Preview](#preview)). It follows Mod 3, because logistics needs a second planet to run between.

### Flagship 3: Automation (Mod 5)

**What it is meant to do.** Programmable automation in the game, built on Stationeers' IC10
language: programmable chips, sensors that read the new simulation (temperature, pressure, gas
composition, a planet's state) and devices that act on it (valves, pumps, compressors).

**Status: first released in Alpha 2** (see [Mod 5](#mod-5-automation--ic10) above). What comes
next for it: the final building art, driving more than a building's first automation input, an
interpreter written independently of Stationeers' own, and readings of the planetary state once
the second flagship has one.

## Following along and reporting issues

- **Releases:** watch the repositories above. Each release is one commit per repository, tagged
  `v<version>`, with its notes in `CHANGELOG.md`.
- **Issues are open.** The repositories have issue forms: a bug report, which asks for the SDK
  version, game build, installed `SimDLL.dll`, enabled mods and `Player.log`, a feature or API
  request, and an [idea request](#suggesting-an-idea). Please file an issue in the repository the problem is in; if you are unsure which, use
  [oni-sdk-docs](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/issues).
- **Pull requests are not accepted yet.** The public repositories are generated from the
  development repositories at each release, so a pull request cannot be merged directly. A fix
  suggested in an issue is ported by hand. Each repository's `CONTRIBUTING.md` explains this.
