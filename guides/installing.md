# Installing and removing

## What is replaced, and why

The SDK replaces one file in the game:

    <install>/OxygenNotIncluded_Data/Plugins/x86_64/SimDLL.dll

`<install>` is the game's folder, for example
`C:\Program Files (x86)\Steam\steamapps\common\OxygenNotIncluded`.

That library runs the world simulation. A managed mod cannot change it, so the only way to give
mods new simulation behaviour is to replace it. No other game file is changed; a backup of the
original and a small record of what was installed are kept beside it. The SDK runs only on the
game's Windows version. The
framework and the mods install as ordinary mods, and the framework can put the library in
place for you (see Installing).

## Deciding whether to trust it

You are replacing a native library that runs inside the game with the same rights as the game.
You should be able to check what you run:

- **The source is public.** Every line of the library is in
  [oni-sim-replacement](https://github.com/Salacious-Oni-Dev/oni-sim-replacement), under the
  MPL-2.0, and nothing in it is closed.
- **You can build it yourself.** The build needs mingw-w64, bash, Python 3.8 or later and an installed copy
  of the game; see that repository's README. A DLL you built from source you have read needs no
  further trust.
- **A released DLL comes with its SHA-256.** Compare it with the file you downloaded before you
  install it.

  Windows (PowerShell):

      Get-FileHash .\SimDLL.dll -Algorithm SHA256

  Linux or WSL:

      sha256sum SimDLL.dll

  If the hash differs from the one on the
  [release page](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/releases), do not install the file.

  The framework mod's copy is `native/SimDLL.dll` in its folder, and `native/SHA256SUMS` beside
  it holds its hash. The framework checks that hash itself before every use and refuses a copy
  that does not match.
- **The framework reports what is loaded.** `SimVersion.Version` is the library's own build
  version (`0.1.<n>+<commit>`, see [Compatibility](compatibility.md)), `SimVersion.FileHash` the
  first seven characters of the SHA-256 of the file the game actually loaded, and
  `SimVersion.FilePath` where that file is.

## Installing

**1. The simulation library**

There are three ways to get it. Use one of them.

*Let the framework provide it (the usual way).* The framework mod carries the library in its
`native/` folder. Install the framework and the mods (see "The framework and mods" below). The first time the game starts
with the framework enabled, a dialog on the main menu explains what the framework will do and
asks whether to use the SDK library. If you accept, then each time the game starts the framework:

- checks that the game's `SimDLL.dll` is the game's own library for a game build this release
  supports, and on any other build shows a notice and changes nothing;
- renames it to `SimDLL.dll.vanilla`, copies its own library in, and checks the copy's SHA-256
  before the game can use it;
- writes `SimDLL.dll.oniframework` beside it, recording what it put there.

With Mod 1 enabled, the game has already loaded its own library by the time the dialog appears,
so after you accept, the framework puts its library in place and asks to restart the game once.
From then on the swap happens during startup and needs no restart.

When the game quits, the framework puts the game's own library back, so between sessions the
game runs its own library. The copy the game had loaded cannot be deleted while the game is
running, so it is renamed to `SimDLL.dll.sdk-old` and deleted at the framework's next start; the
game never loads a file by that name. Your answer is kept in `OniFramework-simdll.txt` in the
game's mods folder; delete that file to be asked again. The framework only swaps the library
before the game has loaded it. If another mod made the game load it first, the framework says so
and the game's own library is used for that session; move the framework to the top of the mod
list. If the library was installed with the zip installer or by hand, the framework leaves it
alone.

*The installer from the release zip.* Each
[release of oni-sim-replacement](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/releases)
carries a zip with the library and an installer for Windows.

1. Download `oni-sim-replacement-<version>.zip` from the release and unpack it. Run nothing from
   inside the zip: the installer needs the other files beside it.
2. Check the unpacked `SimDLL.dll` against the SHA-256 on the release page, as above.
3. Close the game.
4. In the unpacked folder, double-click `install.cmd`.

The installer finds the game through Steam; if it cannot, run `install.ps1 -GamePath "<game
folder>"` from PowerShell. It installs only on a game build the release supports, and on any
other build it stops without changing anything. It keeps the game's own library beside the
replacement as `SimDLL.dll.vanilla`, after checking that the copy is identical.

*By hand.*

1. Close the game.
2. In `<install>/OxygenNotIncluded_Data/Plugins/x86_64/`, rename `SimDLL.dll` to
   `SimDLL.dll.vanilla`. Keep that file; it is the game's own library.
3. Copy the replacement `SimDLL.dll` into the same folder.

**2. The framework and mods**

Each mod is a folder holding its DLL, `mod.yaml` and `mod_info.yaml`. Download them from the
releases of [oni-framework-api](https://github.com/Salacious-Oni-Dev/oni-framework-api/releases)
(`OniFramework-<version>.zip`, holding the `OniFramework` folder) and
[oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods/releases)
(`oni-flagship-mods-<version>.zip`, holding `mod1-thermo-fluid` and `mod2-matter-environment`),
and unpack both. Copy the `OniFramework` folder, then each mod's folder, into the game's local
mods folder:

    Documents/Klei/OxygenNotIncluded/mods/Local/

Start the game, open **Mods**, enable the framework and the mods, and restart when the game
asks. The framework must be listed above the mods: the game loads mods from the top of the list
down, and new mods are added at the bottom. Drag the framework up if it is below them. If you
do not, it moves itself up at the next start: a message box explains what it is doing, the game
restarts once, and a dialog on the main menu names the mods it moved. This is not a crash. A mod
must not carry its own copy of `OniFramework.dll`; the framework is installed once, as its own
mod.

## Removing

**If the framework provides the library,** it is already removed every time the game quits. To
stop using it, disable or unsubscribe the framework, or delete `OniFramework-simdll.txt` and
choose to keep the game's own library when asked. If the game crashed or was ended from Task
Manager while the SDK library was in use, the library stays in place until the framework's next
start, which puts the game's own back if you no longer accept it. If you removed the framework
in the meantime, verify the game files in Steam as described below. Once the framework is gone,
two files of its own may remain, and you can delete both: `SimDLL.dll.sdk-old` in
`<install>/OxygenNotIncluded_Data/Plugins/x86_64/`, and `OniFramework-simdll.txt` in the game's
mods folder.

**If you used the installer or installed by hand:**

1. In the game's **Mods** screen, disable the mods, then the framework, and close the game.
2. Double-click `uninstall.cmd` from the release zip. It puts the game's own `SimDLL.dll` back
   and removes the backup. By hand: delete the replacement `SimDLL.dll` and rename
   `SimDLL.dll.vanilla` back to `SimDLL.dll`.
3. Delete the framework's and the mods' folders from `mods/Local/` if you no longer want them.

In every case, Steam can restore the game's own library: open the game's **Properties**, then
**Installed Files**, and choose **Verify integrity of game files**.

**Saves.** Keep a copy of any save made while the SDK was in use before you remove it. A world
in which no mod stored extension data is saved in exactly the game's own format. A world in
which one did is saved in a newer format version that the game's own library is not built to
read, so keep the framework enabled for those saves. The replacement library's
[save format](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/SAVE-FORMAT.md)
lists the versions.

## After a game update

A game update restores the game's own `SimDLL.dll`. After an installer or by-hand install, the
replacement is simply gone and the game runs unmodified. The framework notices the new build at
the next start: if its release does not support that build, it shows a notice and leaves the
game's own library in use until a framework update adds the build. Before reinstalling by hand,
check [Compatibility](compatibility.md): the game's message layouts can change between builds,
and the library is built against a specific one.
