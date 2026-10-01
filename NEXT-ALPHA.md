# What comes next

This page says what Alpha 1 of the Oxygen Not Included simulation SDK is, what it does not do yet,
what the next releases are expected to bring, and how releases work. Everything below the Alpha 1
sections is a plan, not a promise: scope moves to fit the release date, and anything not ready
waits for the following release.

## How releases work

Releases run on a fixed schedule, about every four weeks after Alpha 1. **The date is fixed; the
scope moves.** Each release ships what has passed its checks by that release's cut-off, and nothing
holds a release back for a feature that is late. A late feature moves to the next release.

Every release has one version number (`0.1.0-alpha.1`, then `0.1.0-alpha.2`, and so on),
tagged in every repository. The [Compatibility](guides/compatibility.md) guide covers numbering,
supported game builds and what a game update means. Each repository's `CHANGELOG.md` records what
changed.

### Cut-offs

- **Scope freeze.** About a week before each release, scope is frozen. Only work that is in the
  development repositories by then can ship; nothing is added to a release after its freeze.
- **The last week is for fixes.** Inside the week before a release, the only work done for it is
  fixing problems that would stop it shipping.
- **New work starts early.** New features start only in the first half of a four-week cycle.
  Anything proposed later is planned for the release after.

### How a candidate is checked before it ships

A change to the simulation library has to pass, at the freeze:

- **The bit-exact comparison with the game's own library.** Recorded scenarios are run through
  the game's own `SimDLL.dll` and through the replacement, and the output must match exactly.
  This is what makes "behaves like the game until a mod uses its extensions" a checked claim
  rather than a hope. A feature that changes behaviour ships off by default or opt-in, so that
  with it off the comparison still matches.
- **The offline suites.** White-box tests of the simulation's kernels, the extension surface and
  the save format, plus fixed-world checks whose cell counts and state digests are recorded and
  must not move unless a change means them to.
- **A performance benchmark.** The simulation's kernels are timed against a recorded baseline,
  on worlds that have been left to settle first, for any change that could affect speed. It is
  not yet an automatic gate.
- **Live runs in the game.** Scripted test scenarios run in a real game with the SDK installed,
  each ending in a pass or fail verdict. The showcase videos in
  [oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods) are recordings of
  such runs. A failure is investigated before the release, and is either fixed or listed as a
  known issue.
- **An install test.** A fresh copy of each repository is built from source, installed, played
  and removed again, and the game's own library must be back in place afterwards.
- **A wording and content check** on everything that goes into a public repository.

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

## Alpha 1

Alpha 1 is an SDK: a replacement simulation library, the API mods call, the first gameplay mods
built on it, and the tools to inspect and develop against it.

| repository | in Alpha 1 |
|---|---|
| [oni-sim-replacement](https://github.com/Salacious-Oni-Dev/oni-sim-replacement) | the replacement `SimDLL.dll`, behaving like the game's own library until a mod uses its extensions: gas mixtures, per-cell properties, per-element attributes, event streams, conduit runs, a grid raycast, mass and energy ledgers |
| [oni-framework-api](https://github.com/Salacious-Oni-Dev/oni-framework-api) | the `OniFramework` mod: the managed API, and delivery of the simulation library with the player's consent |
| [oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods) | Mod 1, Physical Thermodynamics + Fluid Dynamics, and Mod 2, Matter / Environmental Physics |
| [oni-sim-visualizer](https://github.com/Salacious-Oni-Dev/oni-sim-visualizer) | a standalone viewer for the simulation's state, from a recording or a running game |
| [oni-dev-environment](https://github.com/Salacious-Oni-Dev/oni-dev-environment) | a debuggable development copy of the game |
| [oni-sdk-docs](https://github.com/Salacious-Oni-Dev/oni-sdk-docs) | the guides that cover the SDK as a whole |

### Also in Alpha 1

- **Small heat payments land in full.** The game applies each heat payment to a cell as one
  temperature step, so a payment that is small next to the cell's heat capacity comes out a
  little too large or too small, the same way every time. An optional setting keeps each cell's
  remainder for its next payment. It is off by default, which is the game's own arithmetic, and
  Mod 1 switches it on.
- **Mixed-gas temperatures conserve energy.** When gas is added to a cell that holds a mixture,
  the new temperature is blended by heat capacity rather than by mass. This matters only to
  mods that use gas mixtures.

## Known limitations of Alpha 1

Alpha 1 is an alpha. These are the things it does not do yet, or does only roughly, so that
testers know what to expect.

**Platform and installation**

- **Windows only.** The simulation library is a Windows x64 DLL, so the SDK needs the game's
  Windows version. On any other platform the framework leaves the game's own library in use, and
  Mods 1 and 2 cannot do their work.
- **One game build.** Alpha 1 supports game build 744825 only. On any other build the framework
  and the installer leave the game's own library in place.
- **A game update puts the game's own library back.** Install again only with a release that
  lists the new build. See
  [After a game update](guides/installing.md#after-a-game-update).
- **The first acceptance restarts the game once** when Mod 1 is enabled, because the game has
  already loaded its own library by the time the consent dialog appears.
- **Build version, not release number.** The watermark, `FrameworkVersion` and `SimVersion` show
  a build version (`0.1.<n>+<commit>`); the release is the tag. See
  [Compatibility](guides/compatibility.md).

**Saves**

- **A world in which a mod stored extension data is saved in a newer format,** which the game's
  own library is not built to read. Keep a copy of saves before removing the SDK, and keep the
  framework enabled for those saves.

**Simulation and gameplay**

- **A cell still holds one disease.** When two diseases meet, one is destroyed, as in the game
  itself. Keeping several is a candidate for the next alpha (below).
- **Disease is not carried through pipe equalisation** in Mod 1's pipes.
- **Liquids behave as in the game:** a deeper cell simply holds more mass, and there is no
  pressure that rises with depth. Depth pressure is a candidate for the next alpha (below).
- **Gases far above their critical point.** Mod 1's pipes treat condensation by a vapour-pressure
  curve. A gas pipe held at very high pressure can report a condensation temperature above the
  gas's critical point, where no liquid should form. A fix to the model is planned.
- **Balance is not final.** Mod 1 and Mod 2 are demonstrations of what the simulation can do. Their
  numbers, and how hard they make a colony, can change between releases.
- **No planet, logistics or programmable automation yet.** Those are the second and third
  flagships (below), and none of them is in Alpha 1.

**Compatibility with other mods**

- **Mods that replace the simulation library, or that load it before the framework,** stop the
  framework from providing it; the game's own library is then used for that session. Keep the
  framework at the top of the mod list.
- **FastTrack,** the widely used community performance mod, has been run alongside the framework
  and Mods 1 and 2 without errors. It replaces the code path Mod 1 uses to draw its overlays,
  though, and whether Mod 1's overlays still update under it has not been checked yet.

**Development tooling**

- **The debug servers have no authentication.** The framework's debug server, which Mod 1 starts
  with `--oni-debug-inspector` and the visualizer attaches to, listens on every network interface.
  Use it only on a machine and network you trust, and only while you use it. The development copy
  of the game has the same caveat for its debugger.

## Performance

### What Alpha 1 already gains

Every number here was measured on a development machine, on the conditions given. They show the
size of each change; they are not a promise about any particular colony or computer.

**In the simulation library**

- **The simulation frame runs on a worker thread,** as the game's own library does, while the game
  carries on. When this landed, the time the replacement added to each game frame over the game's
  own library fell from 1.73 ms to 0.40 ms on a large test save, and from 2.53 ms to 0.42 ms on a
  mid-sized one. Its output is byte-identical to running the frame on the game's thread. Measured
  later on a large test save with Mods 1 and 2 running, the simulation cost the game's main thread
  about 0.5 to 0.6 ms per simulation tick, 2.7 % of its frame; the rest of the simulation's work
  is hidden on the worker.
- **Compiler optimisation and a baseline instruction set** (`-O3`, and the x86-64-v2 instruction
  set). On a 256 x 384 test asteroid the frame got 7.3 %
  faster, and on a settled, mostly-rock test world 22.1 % faster, with output identical to the
  previous build. A newer instruction set was tried and refused, because it changed results.
- **Each world in a cluster refreshes only its own area.** The game runs the simulation once per
  discovered world, and the replacement used to copy the whole grid for each one. On a test
  cluster cut into twelve worlds, with the same cells to simulate, the frame got 19.6 % faster;
  a single world is unchanged.
- **The pass that tidies empty cells skips them cheaply.** One compiler hint on that pass took it
  from 0.681 ms to 0.276 ms on a twelve-world test cluster, and an idle frame there from 2.143 ms
  to 1.752 ms (18 % faster). No other part of the simulation changed speed, and the output is
  byte-identical.
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

None of this is in Alpha 1.

- **Garbage-collection stutter.** The occasional long freeze in a large colony is the game's
  garbage collector stopping everything to scan a managed heap of several gigabytes: about 216 to
  242 ms a collection on large test saves. A mod can change when collections happen, not what one
  costs. A separate garbage-collection project is in development. It replaces the game's
  collector runtime with one that marks in parallel: on a test save with about 3.3 GB of live
  heap, the part of a collection that stops the game fell from 219.7 ms to about 42 ms, and it has
  passed two-hour soak tests with saves and loads. A companion mod that moves collections to
  moments when they do not show (a pause, losing window focus) is in testing. Because this also
  replaces a game file, the plan is for it to share the SDK's installer, with the same hash
  checks, backup and restore.
- **Where a frame actually goes.** Measured on a large test save with the SDK and Mods 1 and 2:
  the main thread takes about 22 ms a frame, and about 78 % of that is the game's own managed
  per-frame code (8.1 ms in updates, 9.1 ms in late updates). The GPU finishes in under 3 ms, and
  the render thread spends about 18 ms waiting for managed code. The simulation is a small part of
  the frame, so the larger gains are in the game's managed code, and that is where this work is
  looking next.
- **The duplicant and critter AI scheduler.** Over a 4.6-hour session on build 744825 it used
  10.9 % of all wall-clock time, and as much while the game was paused as while it ran, because it
  runs a fixed amount of work per rendered frame and never checks for a pause.
- **Overlay textures.** The game's overlay texture work costs about 1.55 ms a frame on a late-game
  test colony. Only about 0.42 ms of that could move into the simulation library, so the work
  focuses on not recomputing what has not changed (the cache above), and on the change
  information the simulation would need to extend it to more passes.
- **FastTrack.** Any performance claim for the SDK is checked against a game that already runs
  FastTrack, not only against the plain game. On a development copy of the game, FastTrack fails
  without a small fix, which the project's profiling tools carry; with it, FastTrack took a large
  test save from 46.3 to 51.5 fps, all of it from the game's late-update code. Checking that Mod 1's
  overlays still update under FastTrack is planned.
- **Memory and loading.** On a large development save, the managed heap of about 10.85 GB was measured to
  be live data rather than uncollected garbage, so the lever is how much the game keeps, not the
  collector. The heap also grows by several hundred MB with each load of a save within one
  session, with only diagnostic mods enabled, which is being investigated.
- **More simulation speed.** Two further changes are scoped and not built: skipping cells that
  provably cannot change (on a settled test asteroid, heat conduction alone would fall by about
  half), and running the empty-cell pass only where something changed.

## Candidates for the next alpha

Expected about four weeks after Alpha 1. These are candidates; each ships only if it has passed
its checks by the cut-off. The simulation side of the first three is built in development and
going through those checks now.

### More than one disease per cell

- **What players see.** Nothing changes unless a mod switches it on. With it on, several diseases
  can share a cell instead of one destroying the other where they meet, so a contaminated area
  keeps all of what is in it. The germ overlay and everything else in the game still show each
  cell's strongest disease.
- **What modders get.** `OniFramework.MultiDisease`:
  - `SetPolicy(MultiDiseasePolicy.PerCell)` allows up to eight diseases in a cell
    (`MultiDiseasePolicy.Vanilla` is the game's own rule);
  - `TryReadCell(cell, CellDisease[])` reads every disease in a cell: its index, germ count and
    how long it has been there;
  - the `Consumed` event reports the extra diseases a consumer takes in with
    the mass, which the game's own consumed-mass report has room for only one of. (A consumer is a
    pump, a building's intake, or anything else that takes mass out of the world.)

  The extra diseases are saved with the world. The policy itself is not, so a mod sets it once
  per session.
- **Opt-in, off by default.** With it off, the simulation behaves exactly as it does now. It has a
  cost when on: in development benchmarks with two diseases in every infected cell, the gas pass
  takes about 0.7 ms more a frame.
- **How to try it.** No flagship mod switches it on yet, so trying it takes a mod of a few lines
  that calls `SetPolicy` when a world loads.

### An odour field

- **What players see.** Nothing yet. The field is there for gameplay mods to use; the gameplay
  that uses it (duplicants getting grimy, smelling, and reacting to it) is planned for a later
  release.
- **What modders get.** `OniFramework.Odour`, with three channels (`OdourChannel.Funk`, `Stink`
  and `Rot`):
  - `Emit(cell, channel, amount)` puts odour into a gas cell;
  - `Read(cell, channel)` and `Composition(cell, values)` read it back;
  - `SetHalfLife(channel, seconds)` sets how fast each channel fades.

  Odour rides the air: it moves with gas as gas moves, spreads between neighbouring gas cells,
  fades at its own rate, never enters liquid or rock, and leaves when its gas does. The values are
  saved with the world; the rates are set once per session.
- **Always present, empty until a mod emits.** Nothing in the game emits odour, so it costs
  nothing measurable until a mod uses it. With odour in every gas cell of a test world, it costs
  about 1.3 ms a frame.
- **How to try it.** A mod emits odour, for example from a building or a duplicant, and reads it
  back on a hover card or an overlay.

### Liquids with real depth pressure

- **What players see.** In the game, the deeper water is, the more mass each cell holds, and
  pressure exists only as that extra mass. With this switched on, liquid keeps its real density,
  every liquid cell has a pressure that rises with depth, and connected pools level out by
  pressure. A wall holding back deep water feels the weight of the whole column above it.
- **Off by default, for testing.** The first stage is a simulation setting,
  `HydrostaticsEnabled`, off unless switched on. With it off, the simulation's output is
  byte-identical to today's.
- **What modders get.** At first, only the setting and its tuning values, through the existing
  `SimTunables` API. Sealed tanks, siphons and a dedicated API are later stages.
- **How to try it.** Once it ships, set `"HydrostaticsEnabled": 1` in `sim-tunables.json` in the
  framework's folder (see the framework's README) and start a world with water in it.

### Fixes

Fixes to anything reported against Alpha 1, chosen as described under "How fixes are picked".

## Further out

The SDK's long-term plan is five gameplay mods in three flagship releases. Alpha 1 carries the
first flagship (Mods 1 and 2). The other two are further out, and their order and timing are not
fixed.

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

The simulation side of the planet environment is already in the Alpha 1 library. The API that
drives it, and Mod 3 itself, are not.

**Planned, not built:** machines that extract and process the atmosphere, the liquid carbon
dioxide refrigeration loop, dust, hazards, and more planets after Mars.

### Flagship 2: Planetary Logistics (Mod 4)

**What it is meant to do.** Once there is more than one colony, connect them: landing pads, cargo,
storage, a console for orders and trade, and planets that need to import what they lack. It is
built on the game's own rockets rather than a new transport system.

**Status: planned.** No code exists yet. The landed rocket and launch pad Mod 3 already places are
the natural first anchor for landing pads and cargo, and logistics needs a second planet to run
between, so it follows Mod 3.

### Flagship 3: Automation (Mod 5)

**What it is meant to do.** Programmable automation in the game, built on Stationeers' IC10
language: programmable chips, sensors that read the new simulation (temperature, pressure, gas
composition, a planet's state) and devices that act on it (valves, pumps, compressors).

**Status: research done, no code yet.** The study of IC10 is complete: its instruction set of
about 150 instructions, its registers and stack, how it runs a fixed number of instructions per
tick, and how it addresses devices directly, across a data network, or by reference. The finding
is that IC10 itself can be ported closely. The real design work is the device interface, the
equivalent of Stationeers' interface for logic values, through which game buildings would be
read and controlled. The physical state it would read and drive already exists in Mods 1 and 2
(valves, pumps, phase chambers, pipe pressure and stress).

## Following along and reporting issues

- **Releases:** watch the repositories above. Each release is one commit per repository, tagged
  `v<version>`, with its notes in `CHANGELOG.md`.
- **Issues are open.** The repositories have issue forms: a bug report, which asks for the SDK
  version, game build, installed `SimDLL.dll`, enabled mods and `Player.log`, and a feature or API
  request. Please file an issue in the repository the problem is in; if you are unsure which, use
  [oni-sdk-docs](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/issues).
- **Pull requests are not accepted yet.** The public repositories are generated from the
  development repositories at each release, so a pull request cannot be merged directly. A fix
  suggested in an issue is ported by hand. Each repository's `CONTRIBUTING.md` explains this.
