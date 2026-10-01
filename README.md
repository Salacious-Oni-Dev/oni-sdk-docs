# oni-sdk-docs

Guides for the Oxygen Not Included simulation SDK as a whole: what it is, how its parts fit
together, how to install and remove it, which versions work together, and where a mod author
starts.

Each component documents itself in its own repository, versioned with its code. The guides here
cover what no single component can, and link into those references rather than copying them.

**Status: alpha.** Interfaces can still change between releases.

**Windows only.** The simulation library is a Windows DLL, so the SDK needs the game's Windows
version.

## The SDK

| repository | what it is | license |
|---|---|---|
| [oni-sim-replacement](https://github.com/Salacious-Oni-Dev/oni-sim-replacement) | a replacement for the game's native simulation library, `SimDLL.dll`, with an extension surface for mods | MPL-2.0 |
| [oni-framework-api](https://github.com/Salacious-Oni-Dev/oni-framework-api) | the managed API that mods call, installed as its own mod | MIT |
| [oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods) | gameplay mods built only on that API, as working examples | MIT |
| [oni-sim-visualizer](https://github.com/Salacious-Oni-Dev/oni-sim-visualizer) | a standalone viewer for the simulation's state, from a recording or a running game | MPL-2.0 |
| [oni-dev-environment](https://github.com/Salacious-Oni-Dev/oni-dev-environment) | a debuggable development copy of the game, with its own data and Klei's debug tools working again | MIT |

## Guides

| guide | for |
|---|---|
| [Getting started](guides/getting-started.md) | what the SDK does, how the three parts depend on each other, and the order to install them |
| [Installing and removing](guides/installing.md) | the file that is replaced, how to check it, and how to get back to the unmodified game |
| [Compatibility](guides/compatibility.md) | how releases are numbered, which game build each release supports, and what a game update means |
| [Writing a mod](guides/mod-authors.md) | referencing the framework, declaring the API level you need, and where each reference lives |
| [Visualizing the simulation](guides/simviz.md) | recording a corpus from your own game, and attaching the visualizer to a running game |

## What comes next

Releases come about every four weeks after Alpha 1. The date is fixed and the scope moves: each
release ships what has passed its checks by its cut-off, and anything late moves to the next one.

**Candidates for the next alpha.** The simulation side of each is built in development and going
through its checks:

- **More than one disease per cell.** Several diseases can share a cell instead of one destroying
  the other where they meet. Off unless a mod switches it on.
- **An odour field.** Three odour channels that spread through the air, for gameplay mods to
  build on: duplicants getting grimy, smelling, and reacting to it come later.
- **Liquids with real depth pressure.** Liquid keeps its real density, pressure rises with depth,
  and connected pools level out by pressure. Off by default at first, as a simulation setting.
- **Fixes** to anything reported against Alpha 1.

**Performance.** Work under way on the game's garbage-collection stutter (on a large test save,
the pause of a collection fell from about 220 ms to about 42 ms in development), on the AI
scheduler that costs time even while the game is paused, and on further simulation speed-ups.

**Further out.** The rest of the five-mod plan:

- **Dynamic Planet (Mod 3).** Mars as a world of its own: the colony lands in a crippled rocket on
  a thin carbon dioxide atmosphere, with a day and night swing from about 293 K to 220 K, a sun
  that moves through the Martian year, and an atmosphere that is a resource. Early versions run in
  development builds.
- **Planetary Logistics (Mod 4).** Landing pads, cargo, storage and trade between colonies, built
  on the game's own rockets. Planned.
- **Automation (Mod 5).** Programmable automation built on Stationeers' IC10 language, with
  sensors and devices that read and drive the new simulation. Research done, no code yet.

[What comes next](NEXT-ALPHA.md) has all of this in detail: how releases are planned and checked,
everything Alpha 1 contains and its known limitations, the performance measurements, what each
candidate gives players and modders, and the status of each future mod.

## License

MIT, see `LICENSE`.

## Credits

Oxygen Not Included is developed and published by Klei Entertainment. This project is not
affiliated with or endorsed by Klei.

Development of this project uses AI coding assistants. All changes are reviewed and released
by the maintainer.
