# Visualizing the simulation

[oni-sim-visualizer](https://github.com/Salacious-Oni-Dev/oni-sim-visualizer) is a standalone
program that draws the simulation's state: element, temperature, mass, phase, flow and any
extension property a mod has registered. It is never loaded into the game. It works from one of
two sources:

- **A recorded corpus.** The program loads `SimDLL.dll` itself, replays the messages your game
  sent to it, and runs the simulation on its own. You can step forwards and backwards.
- **A running game.** The program reads the live colony over a socket from the framework's
  debug server. It goes forwards only.

This guide covers getting each source. The visualizer's README covers building it, its views,
keys and flags, and its check suite.

## Recording a corpus

A corpus holds the real messages the game sent to its simulation library. You record it from
your own game with the passthrough shim in `oni-sim-replacement/shim/`.

1. Remove or disable every mod. A corpus recorded with mods holds messages a stock game never
   sends.
2. Build the shim with `shim/build.sh` in your `oni-sim-replacement` clone.
3. In `<install>/OxygenNotIncluded_Data/Plugins/x86_64/`, rename the game's `SimDLL.dll` to
   `SimDLL_orig.dll` and copy the built shim there as `SimDLL.dll`.
4. Start the game and **load a save as the first thing you do**. Do not start a new game first.
5. Once the colony is on screen, quit the game.
6. In the same folder, delete the shim's `SimDLL.dll` and rename `SimDLL_orig.dll` back to
   `SimDLL.dll`.
7. Move `sim_corpus.bin` out of the folder to wherever you keep it, and delete the three log
   files the shim leaves beside it: `sim_notes.log`, `sim_shim.log` and `sim_timing.log`.

The order matters because the shim records the **first** simulation session the game starts.
When you start a new game, that first session is world generation, and a corpus of it boots
onto a world the simulation has not yet published. The visualizer warns about such a corpus,
and its check suite refuses one. Loading a save first records the session you want.

A corpus contains data from the game, including its element and disease tables. Keep it for
your own use and do not share it.

Then open it:

```sh
./build/simviz.exe --corpus sim_corpus.bin --seed 7 --view temperature --window
```

`--seed` pins the simulation's random stream, so two runs of the same command match. Without
it, a loaded world seeds from the clock and runs drift apart.

## Attaching to a running game

The attached source needs the framework and Mod 1 installed (see
[Installing and removing](installing.md)), and the game started with one extra launch option:

```
--oni-debug-inspector
```

In Steam, set it under the game's **Properties > General > Launch Options**. With it,
Mod 1 starts the framework's debug server on port 9788 when a colony loads. Then:

```sh
./build/simviz.exe --attach 127.0.0.1:9788 --view temperature --window
```

From WSL, `127.0.0.1` is not the Windows host. Use the address of the WSL virtual network
adapter on the Windows side instead.

**The debug server has no authentication and listens on every network interface.** Use the
launch option only on a machine and network you trust, and remove it when you are done.

## What to try first

- `--probe x,y` prints one cell's element, mass, temperature and every registered extension
  value. Coordinates are the image's, with `y` counted from the top, so a feature seen in a
  snapshot can be probed where it was seen.
- `--ext-list` names every per-cell extension property and says how many cells hold something
  other than its default, which is the quickest way to see whether a mod's property is doing
  anything.
- In the window, `tab` opens the control panels. The **help** tab shows the command line that
  reproduces what is on screen, so a view put together by clicking can go into a bug report.
