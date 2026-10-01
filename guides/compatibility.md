# Compatibility

## One version for the whole SDK

Every release carries one version number. Releases are numbered with SemVer and a pre-release
tag: `0.1.0-alpha.1`, `0.1.0-alpha.2`, and so on, then `0.1.0-beta.N`, then `0.1.0`. Each
repository tags the release as `v<version>`, and each mod's `mod_info.yaml` carries it.

The simulation library and the framework also report a build version of their own,
`0.1.<n>+<commit>`, in the watermark and through `SimVersion` and `FrameworkVersion`. Its `0.1`
is the API level and the commit names the exact source it was built from. It is not a release
number: the release is the tag.

**Install components from the same release.** A framework from one release and a library from
another is not a supported combination, even when both happen to load.

**Windows only.** The simulation library is a Windows DLL, so the SDK runs only on the game's
Windows version: the framework's library swap, the mods, the visualizer and the development
environment all need it.

## The API level

A mod does not check the SDK version. It checks the framework's **API level**, `MAJOR.MINOR`,
which moves only when the public API changes:

- **MINOR** goes up when the API gains something a mod can call. A mod built against `0.3` works
  with `0.4`.
- **MAJOR** goes up when something a mod already calls changes shape. A mod built against `1.x`
  does not work with `2.x`, in either direction.

The API level starts at `0.1`. While the SDK is in alpha, the example mods in each release
declare that release's API level. See [Writing a mod](mod-authors.md) for how a mod declares the
level it needs.

## Releases

| SDK version | API level | game build | tag |
|---|---|---|---|
| 0.1.0-alpha.1 | 0.1 | 744825 | `v0.1.0-alpha.1` |

## Game updates

The simulation library exchanges fixed-layout messages with the game. Those layouts belong to
the game, so the library does not carry them: its build reads them from the installed game and
checks the size of every one. A game update that changes a layout therefore stops the library
from building, rather than letting it misread messages at run time.

What that means in practice:

- **A released DLL supports the game build in the table above.** On a different build it may
  work, or the game may misbehave. The framework and the installer both check the game's own
  `SimDLL.dll` against the builds their release supports, and on any other build they leave the
  game's own library in use. The game's own library is always one Steam file check away
  (see [Installing and removing](installing.md)).
- **Building from source** against your installed game checks that build's layouts. If the build
  succeeds, every message layout the library uses matches your game.
- The framework and the mods are managed code. They follow the game's managed API like any other
  mod, and their `mod_info.yaml` states the oldest game build each one supports.
