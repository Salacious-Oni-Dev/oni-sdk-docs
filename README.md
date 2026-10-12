# oni-sdk-docs

*Oxygen Not Included: now with actual reasons for the oxygen to be not included.*

Guides for the Oxygen Not Included simulation SDK as a whole: what it is, how its parts fit
together, how to install and remove it, which versions work together, and where a mod author
starts.

Each component documents itself in its own repository, versioned with its code. The guides here
cover what no single component can, and link into those references rather than copying them.

**Status: alpha.** Interfaces can still change between releases.

**Windows only.** The simulation library is a Windows DLL, so the SDK needs the game's Windows
version.

## Alpha 2 (current release)

Alpha 2 (`0.1.0-alpha.2`) adds liquid depth pressure, several diseases per cell, an odour field,
smoke, fire and the buildings that use it, and rooms whose air condenses and exchanges heat. The
flagship mods gain the Active Vent, the Steam Room, the Campfire, the Fire Sprinkler and more, and a
new mod, Mod 5, brings Stationeers' IC10 programmable chips into the game. No Alpha 1 interface is
removed or changed.

[Changelog](CHANGELOG.md) ·
[Release notes](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/releases/tag/v0.1.0-alpha.2) ·
[What is in Alpha 2, and its known limitations](NEXT-ALPHA.md#alpha-2)

## The SDK

| repository | what it is | license |
|---|---|---|
| [oni-sim-replacement](https://github.com/Salacious-Oni-Dev/oni-sim-replacement) | a replacement for the game's native simulation library, `SimDLL.dll`, with an extension surface for mods | MPL-2.0 |
| [oni-framework-api](https://github.com/Salacious-Oni-Dev/oni-framework-api) | the managed API that mods call, installed as two mods: `OniFramework.Library` (needs no SDK simulation library) and `OniFramework` (the simulation half) | MIT |
| [oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods) | gameplay mods built only on that API, as working examples | MIT, except Mod 5's IC10 interpreter (see its NOTICE) |
| [oni-sim-visualizer](https://github.com/Salacious-Oni-Dev/oni-sim-visualizer) | a standalone viewer for the simulation's state, from a recording or a running game | MPL-2.0 |
| [oni-dev-environment](https://github.com/Salacious-Oni-Dev/oni-dev-environment) | a debuggable development copy of the game, with its own data and Klei's debug tools working again | MIT |
| [oni-sim-vanilla](https://github.com/Salacious-Oni-Dev/oni-sim-vanilla) | a plain `SimDLL.dll` that behaves like the game's own, with no extensions and every part documented; frozen, and not used by the SDK | MPL-2.0 |

Mods built on the SDK need one of two framework mods, both in the
[oni-framework-api](https://github.com/Salacious-Oni-Dev/oni-framework-api) release: a mod that uses
the replacement simulation library needs `OniFramework` together with `OniFramework.Library`, which
it is built on; a mod that does not needs only `OniFramework.Library` 1.3 or later, and runs on the
game's own `SimDLL.dll`.

## Guides

| guide | for |
|---|---|
| [Getting started](guides/getting-started.md) | what the SDK does, how the three parts depend on each other, and the order to install them |
| [Installing and removing](guides/installing.md) | the file that is replaced, how to check it, and how to get back to the unmodified game |
| [Compatibility](guides/compatibility.md) | how releases are numbered, which game build each release supports, and what a game update means |
| [Writing a mod](guides/mod-authors.md) | referencing the framework, declaring the API level you need, and where each reference lives |
| [Visualizing the simulation](guides/simviz.md) | recording a corpus from your own game, and attaching the visualizer to a running game |

## How it is checked

Before every release, in the development repositories:

- the replacement library is run against the game's own library, on recorded scenarios, and the
  output has to match exactly, except for changes that a release names (Alpha 2 names one);
- offline suites test the simulation's kernels, the extension surface and the save format;
- scripted runs in a real game end in a recorded pass or fail, and the showcase videos are
  recordings of those runs;
- each repository is built from source, installed, played and removed again.

The details are in [How a candidate is checked before it ships](NEXT-ALPHA.md#how-a-candidate-is-checked-before-it-ships).

## What comes next

Releases follow a regular cadence, currently a trial of about ten days. Each release ships what
has passed its checks by its cut-off; anything late moves to the next one.

- **Alpha 2:** shipped, see [above](#alpha-2-current-release).
- **Preview:** built in development, not in this release: a first trade route between planets, a
  chlor-alkali cell, an in-game element reference, and art that
  reloads into a running game. Videos are attached to the release. See [Preview](NEXT-ALPHA.md#preview).
- **Further out:** the rest of the five-mod plan: Dynamic Planet (Mars) and Planetary Logistics,
  and what comes next for Automation (Mod 5, first released in Alpha 2). See [Further out](NEXT-ALPHA.md#further-out).

Have an idea? Open an [idea request](NEXT-ALPHA.md#suggesting-an-idea). Make building art? See
[Art wanted](ART-WANTED.md).

## License

MIT, see `LICENSE`.

## Credits

Oxygen Not Included is developed and published by Klei Entertainment. This project is not
affiliated with or endorsed by Klei.
How this project follows Klei's modding guidelines, rule by rule: [COMPLIANCE](COMPLIANCE.md).

Development of this project uses AI coding assistants. All changes are reviewed, tested and
released by the maintainer.
