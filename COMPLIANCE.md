# Klei's modding guidelines: how this project follows them

*Oxygen Not Included: now with actual reasons for the oxygen to be not included.*

This page states, rule by rule, how the SDK and its mods follow Klei's modding guidelines:

- Klei forums: https://kleiforums.com/forums/topic/116697-modding-guidelines/
- Klei support: https://support.klei.com/hc/en-us/articles/360029556052-General-Mod-and-UGC-Guidelines

Oxygen Not Included is developed and published by Klei Entertainment. This project is not affiliated
with or endorsed by Klei. If you think anything here is wrong, open an issue; we would rather fix it
than argue about it.

## The rules, and what we do

**Mods that make use of other people's work without credited permission.**
Everything in these repositories is written by the project unless stated in a repository's NOTICE
file. The design of several systems follows another game, Stationeers (RocketWerkz), as a reference;
where a mechanism follows theirs, the code comment names it. One part goes further and is stated
openly: Mod 5's IC10 interpreter follows Stationeers' own closely, so that Stationeers programs run
unchanged (see Mod 5 below). No other Stationeers code, text or data is included. The Rephysicalized mod by MitBoy42 is recommended alongside our mods and is not bundled; our
compatibility changes are offered back in a public fork of his own repository, with his permission
(permission given in writing on 2026-10-11; the record is held privately by the project owner). The
smoke system came from an idea by Kaz, credited in the README. Third-party libraries are listed with
their licences in each NOTICE. Harmony, Unity Doorstop and Mono.Cecil are obtained at build or install
time and are not redistributed. The two exceptions are named in their repositories' NOTICE files: the
simulation visualizer includes the DejaVu fonts, and its build copies the Qt and MinGW runtime
libraries beside it; oni-gc-runtime releases a Mono runtime rebuilt from Unity's published source,
with its licences. Test saves contributed by players are credited only with the contributor's consent.

**Mods that violate the Steam Subscriber Agreement or Klei's Content Guidelines.**
Every Workshop item carries a title image, screenshots and a full description of what it changes, and
states its alpha status. Nothing is uploaded that is meant for personal use only.

**Mods intended to circumvent metagame entitlements (skins, item drops, DLC, product ownership).**
The SDK needs a legitimately installed copy of the game: the build reads the game's message layouts
from the installed game, and the installer refuses a game build it does not list in
`supported-builds.txt`. No DLC content is added, copied, enabled or unlocked. The only DLC-related
code reads which DLCs are active so that saves record the correct DLC ids and the diagnostic build
stamp can name them; nothing checks, grants or bypasses ownership. Our own elements are registered
with an empty DLC id. Outside the SDK, oni-time-travel recolours the bolt art of the player's own
installed Bionic Booster Pack when the game loads; without that DLC it draws no bolts, and nothing
of the DLC is copied, shipped or unlocked.

**Mods that contain copyrighted material you do not own or have permission to use.**
No SDK repository contains any file of the game: no Klei source code, game binaries or game data,
and no game assets apart from the one piece of art named under Mod 5 below. The simulation library is an independent implementation written to behave like the game's
own, informed by the game's observable behaviour and the symbol file Klei ships beside its library.
Where a comment names one of the game's own functions or types, it uses the name from that shipped
symbol file or the game's managed code, as attribution. Recorded footage and screenshots in the
READMEs are of the game running with these mods, as in any mod's Workshop page. Two exceptions are
stated openly: Mod 5 and oni-perf-mod, below.

**Art or assets not created by the mod creator must link to their permission.**
The SDK repositories ship little art in this release: Mod 2's Campfire, drawn by the project, and
Mod 5's buildings, AI placeholder art to be replaced (see Mod 5 below). A building that has no art of its own yet
wears one of the game's own animations, loaded from the player's installed game at run time and never
copied into a repository. Art drawn for the project goes in the mod's `anim/assets/` folder. If any
third-party asset is ever added, its permission link goes in the same folder and in NOTICE.

**Mods with obfuscated or otherwise ambiguous code.**
Nothing is obfuscated. Every binary we ship is built from the public source at a tagged commit, with
build instructions. Every native library we release (the simulation library and the GC runtime) has
its SHA-256 published on its release page, the installers check it, and the installing guide shows how
to check a file by hand. Replacing a native library is unusual for a mod, so the guides document
exactly which file is replaced, where the backup is kept, and how to undo it (including Steam's
Verify integrity of game files). Players and Klei staff can read every line that runs.

**The framework library.** `OniFramework.Library`, the part of the framework that does not need the
SDK simulation library, is MIT licensed like the rest of the framework and ships in the same
repository (oni-framework-api) as its own mod. It contains no native code, and it never loads,
replaces or calls the simulation library: its build fails if a source file names it. Its release
carries one non-code file, the `onilibrary-shaders` Unity asset bundle, built from the project's own
shader source in `OniFramework.Library/shaders/`; it holds no Klei or Unity asset. The framework's
folder likewise carries `oniframework-shaders` (the fire and smoke shaders), built from
`OniFramework/shaders/`, also with no Klei or Unity asset.

**Mods that contain paywalls or paid content, including supporter early access.**
Everything is free and open source (MIT for the framework, mods and docs; Mozilla Public License 2.0
for the simulation libraries and the visualizer). There is no paid tier, no early access, and no
content or build that is given in exchange for money. Public releases reach GitHub and the Workshop at
the same time with the same content. Donations are accepted and nothing is given in exchange for them:
no content, no build, no access and no early access.

**Modding etiquette.**
Public releases are complete and working at the level their status says (alpha, with the known gaps
listed in each release). We do not reupload or modify other people's mods; the Rephysicalized fork is a
fork on GitHub of the author's public repository, used to propose changes back to him, and never
publishes a build. No assets from other games or media are included.

## Mod 5 (oni-flagship-mods)

Mod 5, Automation / IC10, is new in Alpha 2. Its IC10 interpreter (`mod5-automation-ic10/Ic10/`)
follows the IC10 chip of Stationeers closely and carries Stationeers' names and numbers for its logic
types, so that programs written for Stationeers run unchanged. Those parts are not under the MIT
licence, remain the property of RocketWerkz, are named in the repository's NOTICE, and will be
replaced by independently written code in a later release. The IC Sorter's picture includes the
game's own straight conveyor rail; that rail is Klei's art, is named in the same NOTICE, and will be
redrawn. All of Mod 5's building art is placeholder art made with AI tools, labelled as such, and
will be replaced.

## The mods in Salacious-Oni-Mods

These mods are released under the same licence and the same no-paywall terms as the SDK.

**oni-perf-mod:** three files of its optional `fastLoad`, which is off by default, follow the game's
own load code closely. They are excluded from the MIT licence, named in the repository's NOTICE and in
each file's header, and are to be replaced by independently written code. Every other lever patches
the game at run time through Harmony.

**oni-gc-runtime:** the runtime is Unity's Mono, built from the same published source commit as the
game's own, with its garbage collector changed to mark on several threads. The changes are the patches
in the repository, and the licences of the code it is built from are in its NOTICE.

**oni-backrooms:** the Kennel art is by MitBoy42, provided by him for this mod and credited in the
mod's README with a link to his work and to his permission. All other art, sound and text is the
project's own. No assets from any other game or media are included.

**oni-time-travel:** the building sprite is a placeholder that was generated with an AI image tool; it
is disclosed as such in the README and on the Workshop page and will be replaced by original art. The
time-travel shader and screen effects are the project's own code. All other art, sound and text is the
project's own. No assets from any other game or media are included.
