# Getting started

## What the SDK is

Oxygen Not Included runs its world simulation (gases, liquids, heat, phase changes) in a native
library, `SimDLL.dll`. Managed mods can read what that library publishes and send it the
messages the game already sends, but they cannot change how it works. Anything the library does
not already do is out of reach.

The SDK removes that limit in three layers:

1. **A replacement simulation library** (`oni-sim-replacement`). With no mod using its
   extensions, it aims to behave exactly like the game's own library. Mods can then switch on
   added behaviour: several gases in one cell, extra per-cell properties and per-element
   attributes that the simulation carries and saves, per-frame event streams, phase change
   inside pipes, a grid raycast, and mass and energy ledgers.
2. **A managed framework** (`oni-framework-api`). A mod that other mods depend on. It wraps the
   library's extension surface in a C# API, reports which library and which framework a game is
   actually running, and checks that a mod gets the API level it was built against.
3. **Example mods** (`oni-flagship-mods`). Gameplay built only on the framework's public API,
   to show what the extensions make possible and to serve as reference code.

The SDK runs only on the game's Windows version; see [Compatibility](compatibility.md).

## How the parts depend on each other

```
  your mod    example mods
       \         /
        framework          (a mod; OniFramework.dll)
            |
     replacement SimDLL    (replaces one file in the game)
            |
          the game
```

- A mod that uses the SDK references the framework and never calls the simulation library
  directly.
- The framework loads on the game's own library too. There it reports that the replacement is
  absent (`SimVersion.IsCustom` is false). A call that only sends the simulation a setting is
  ignored; a call that must return a physical quantity has no safe default, so it throws rather
  than return a number a caller might use. Surfaces that are purely managed still work. A mod
  that needs the replacement checks `SimVersion.IsCustom` before using it.
- The example mods require the replacement library and the framework.

## Install order

1. The replacement `SimDLL.dll`. The framework mod carries it and, once you accept, puts it in
   place each time the game starts; the release zip's installer is the other way. See
   [Installing and removing](installing.md).
2. The framework mod, from its release zip, enabled in the game's Mods screen.
3. Any mod that uses it, enabled and listed **below** the framework in the Mods screen. The game
   loads mods from the top of that list down. If the framework finds itself below a mod that uses
   it, it moves itself up and restarts the game once, with a message before and after.

Remove them in the reverse order.

## Next

- A player: [Installing and removing](installing.md), then [Compatibility](compatibility.md).
- A mod author: [Writing a mod](mod-authors.md).
