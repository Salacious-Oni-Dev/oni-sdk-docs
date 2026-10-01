# Writing a mod

This guide is the path from an empty project to a mod that uses the SDK. The details of each
surface live with the code that implements it; the links below point at the release tag, so
they describe the same code you install.

## 1. Install the SDK

Install the replacement library and the framework as described in
[Installing and removing](installing.md). You need both to run a mod that uses the extensions,
and the framework's DLL to compile against.

Avoid calling into the simulation library from your `OnLoad`. The Steam game does not load it
until a world starts, and the framework can only put the SDK library in place while it is
unloaded. A call at `OnLoad` loads the game's own on a player's first start, so accepting the
framework's offer then needs one restart. Mod 1 does this today, which is why accepting with
Mod 1 enabled asks for one restart.

## 2. Reference the framework, and do not bundle it

Reference `OniFramework.dll` with **Copy Local off**, so that your build output does not contain
a copy of it:

```xml
<Reference Include="OniFramework">
  <HintPath>path\to\OniFramework.dll</HintPath>
  <Private>false</Private>
</Reference>
```

Mono loads one assembly per simple name. If two mods each ship `OniFramework.dll`, whichever
loads first is used by every mod in the process, and a mod built against the other copy fails
with a `TypeLoadException` naming a type that plainly exists. The mod that fails is usually not
the one that bundled the copy. Merging the framework into your assembly does not help either:
the types then exist twice, and they cannot be passed between mods.

In your `mod.yaml` description, tell players that the mod needs the framework installed. The
framework asks once, on the main menu, whether to use the SDK library; a player who declines
runs your mod on the game's own library (see section 6).

## 3. Declare the API level you need

Check the framework's API level in `OnLoad`. Declare the lowest level that has everything you
call:

```csharp
using HarmonyLib;
using KMod;
using OniFramework;

public class MyMod : UserMod2
{
    public override void OnLoad(Harmony harmony)
    {
        base.OnLoad(harmony);
        if (!FrameworkVersion.Require(0, 1, "MyMod"))
            return;   // too old, or another major version: degrade or stop
        // ...
    }
}
```

`Require` never throws. It returns false and logs a line naming both versions, and the decision
to degrade or stop stays with your mod. It also checks that only one copy of the framework is
loaded and names every copy's path if not.

A mod that registers its own per-cell properties or element attributes uses the overload that
also returns an `ExtOwner`, the token that names everything the mod registers:

```csharp
if (!FrameworkVersion.Require(0, 1, "MyMod", out ExtOwner owner))
    return;
```

## 4. Check for the replacement library

The framework also loads on the game's own `SimDLL.dll`. Before using anything that needs the
replacement, check:

```csharp
if (!SimVersion.IsCustom)
{
    // The game's own simulation library is loaded. Extensions are unavailable.
}
```

## 5. Find the surface you need

| to | read |
|---|---|
| see how a frame is ordered, and which extension messages exist | [Extension points](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/EXTENSION-POINTS.md) |
| store your own data per cell or per element, and have it saved | [Extension registries](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/EXT-REGISTRY.md) |
| hold several gases in one cell | [Gas mixtures](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/GAS-MIXTURES.md) |
| know what the game reads from the simulation each frame, and what a mod may change | [Projection](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/PROJECTION.md) |
| check that your mod conserves mass and energy | [Conservation ledgers](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/LEDGERS.md) |
| know what a save holds and what a load restores | [Save format](https://github.com/Salacious-Oni-Dev/oni-sim-replacement/blob/v0.1.0-alpha.1/docs/SAVE-FORMAT.md) |
| call any of the above from C# | the [framework's README](https://github.com/Salacious-Oni-Dev/oni-framework-api/blob/v0.1.0-alpha.1/README.md) and the XML documentation on each public type |
| see a complete mod built on the API | [oni-flagship-mods](https://github.com/Salacious-Oni-Dev/oni-flagship-mods/tree/v0.1.0-alpha.1) |

The framework's API is the supported surface. The library's C ABI (`abi/sim_ext_api.h` in
oni-sim-replacement) is documented for completeness and for tools, but a mod that calls it
directly bypasses the framework's version and single-copy checks.

## 6. Test without the replacement

Run your mod once on the game's own `SimDLL.dll` (see [Installing and removing](installing.md)).
It should load, log that the replacement is absent, and leave the game playable. Players will
install mods before they install the library, and a mod that breaks the game in that state gets
the blame for it.
