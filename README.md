# How to Create FiveM Assets with Sollumz 2.9.0

> **A free beginner's guide to creating GTA V and FiveM assets with Sollumz 2.9.0 and Blender.**
> Learn the current setup, build your first Drawable, add GTA materials and collisions, and export correctly for FiveM Legacy or Enhanced.

Sollumz has changed a lot.

Version 2.8 introduced native GTA V binary support and removed a huge amount of the old XML conversion dance.
**Sollumz 2.9.0 builds on that foundation and turns Blender into a much more complete GTA V asset-production environment.**

The importer and exporter have been unified, GTA V Legacy and Enhanced are now explicit export targets, YTDs and YMAPs have much deeper native workflows, and Sollumz now includes tools for things like asset libraries, map placement, presets and MLO instances.

The problem is that a lot of tutorials still show older menus, older export flows, or workflows involving versions of Sollumz that simply don't behave the same way anymore.

This guide covers the bit every creator needs first:
**make an asset in Blender, give it GTA-compatible materials and collision, and get a valid game-ready file out of Sollumz.**

No 3DS Max dependency.
No blindly following a six-year-old YouTube video.
No pretending XML is still mandatory for everything.

---

## What This Free Guide Covers
By the end of this guide you'll understand:
* Installing Sollumz 2.9.0
* Setting Blender up correctly
* The basic Sollumz hierarchy
* Creating a Drawable (`.ydr`)
* GTA V shader materials
* Embedded textures
* Collision meshes
* Collision materials
* Native binary export
* CodeWalker XML fallback
* Gen8 vs Gen9 exports
* FiveM resource basics

This is deliberately a **beginner guide**.

The full guide goes considerably further into MLO creation, rooms, portals, doors, YTYPs, YMAPs, native YTD workflows, world placement, vehicles, asset libraries, presets, optimization, commercial packaging and troubleshooting.

More on that later.

---

# What's New in Sollumz 2.9.0?
If you already used Sollumz 2.8, the biggest thing to understand is that **2.9 isn't just 2.8 with some extra buttons**.
The direct binary workflow introduced in 2.8 is still here, but the surrounding production workflow has grown considerably.

Sollumz 2.9 includes major improvements around:
* YMAP creation and world placement
* Map groups and containers
* Native YTD texture dictionaries
* MLO instances
* GTA V asset libraries
* Expanded presets
* Vehicle window tooling
* Gen8 and Gen9 export workflows
* Blender 5.x compatibility

The import/export terminology has also changed.
Instead of tutorials telling you to look for old `Import Sollumz` or individual format exporters, the two names you should remember are:

**File > Import > Import RAGE Assets**
and
**File > Export > Export RAGE Assets**

Sollumz determines what to export from the asset hierarchy you've created.

A Drawable root becomes a YDR.
A Fragment becomes a YFT.
A Bound Composite becomes a YBN.

The hierarchy isn't just organization anymore.
**It tells Sollumz what the asset actually is.**

---

# Legacy vs Enhanced
FiveM now exists against two different GTA V generations.

### GTA V Legacy
Uses **Gen8** assets.

### GTA V Enhanced
Uses **Gen9** assets.

This matters when exporting.

A beautiful model exported for the wrong generation is still the wrong file for the server trying to load it.
Sollumz 2.9 can export Gen8, Gen9, or both.

If you're releasing assets publicly or commercially, supporting both is considerably easier than maintaining two separate Blender projects.
You build the asset once and export the appropriate generations.

---

# What You'll Need

## Blender
You'll need:
**Blender 4.2 or newer**

This guide is written around:
**Blender 5.2**
You can download Blender from:
https://www.blender.org/

Older supported Blender versions should follow essentially the same asset workflow, although Blender's own interface may look slightly different.

---

## Sollumz 2.9.0
Install the stable version of Sollumz 2.9.0.

Official project:
https://github.com/Sollumz/Sollumz

Official documentation:
https://docs.sollumz.org/

For production work, stick to the stable release rather than building your entire workflow around a development version that may change underneath you.

---

## PyMateria
Sollumz can use **PyMateria** to directly read and write supported GTA V binary formats.
On Windows, Sollumz will offer to install the dependency for you.

You want this if you're using the native workflow.
Without PyMateria, Sollumz can still work using the **CodeWalker XML** workflow.

### Windows
Use PyMateria and native binary import/export where supported.

### Linux / macOS
Use the CodeWalker XML workflow where native PyMateria support is unavailable.
There are also still some asset formats where CodeWalker XML remains useful even on Windows.

So:
**XML isn't dead.**

It just isn't mandatory for every asset anymore.

---

# Installing Sollumz
Install and enable Sollumz using the current stable package.

Once installed:
1. Restart Blender.
2. Open the 3D Viewport.
3. Press `N`.
4. Look for the **Sollumz Tools** tab.
5. Open the **File > Import** menu.
6. Confirm **Import RAGE Assets** exists.
7. Open **File > Export**.
8. Confirm **Export RAGE Assets** exists.

If they're there, Sollumz is loaded.
If you're on Windows and want native binary support, make sure PyMateria is installed too.

---

# Set Your Scale First
GTA V works nicely with a simple rule:
**1 Blender unit = 1 metre**

Do this properly from the beginning.

Fixing scale after you've already built:
* geometry
* collisions
* doors
* portals
* entities
* lights
* placement

is significantly less entertaining.
A normal interior door being twelve metres tall is funny exactly once.

---

# Getting Familiar With Sollumz
Most of your work happens through the **Sollumz Tools** panel in Blender's sidebar.
Press:
`N`
to show the sidebar if it isn't visible.

You'll use Sollumz Tools for things such as:
* Drawables
* Materials
* Bounds
* Collisions
* Fragments
* MLOs
* Texture Dictionaries
* Maps
* Asset Libraries

Don't worry about learning all of them yet.
We're starting with the most fundamental GTA V asset:

# The Drawable

---

# Creating Your First Drawable
A **YDR** is a GTA V Drawable.

Drawables are used for things like:
* buildings
* furniture
* scenery
* props
* static models

For this example, create or import a simple mesh.
It doesn't need to be impressive.

A basic wall, crate, table or simple building shell is enough.
The goal here is to understand the pipeline.

---

## Step 1: Prepare Your Mesh
Create or import your model.
Make sure the scale looks sensible.

Then apply its transforms:
`Ctrl + A`
Choose:
**All Transforms**

This clears out unexpected scale and rotation values before Sollumz starts building game-specific structures around the object.
You should also give the object a sensible name.

Instead of:
`Cube.017`

try something like:
`ajd_test_wall`
or
`prop_workbench_01`
Your future self will appreciate this once the Outliner contains 200 objects that Blender helpfully named Cube.

---

# Converting the Mesh to a Drawable
Select your model.

Open:
**Sollumz Tools**

Find the Drawable creation tools and convert the selected object into a Drawable.
Sollumz will create the hierarchy GTA V expects.

Conceptually you'll end up with something similar to:
```text
Drawable
└── Drawable Model
    └── Geometry
```

The exact visible hierarchy can vary depending on what you're building, but the important thing is that the asset now has a **Drawable root**.
That root identifies the asset as something Sollumz can export as a YDR.

---

# Materials and GTA V Shaders
A normal Blender material is not automatically a GTA V material.

GTA V uses its own shader system.
Select your Drawable geometry and open Sollumz's shader tools.

For ordinary surfaces, one of the most common shaders you'll encounter is:
**NORMAL**

This is useful for typical surfaces using diffuse and normal textures.

Other shaders exist for different jobs, including:
* basic surfaces
* specular surfaces
* glass
* vehicles
* emissive materials
* specialized GTA effects

For your first asset, don't choose the most complicated shader because it has the most exciting name.
Use the simplest shader that actually does what the surface needs.

---

# Adding Textures
Switch to Blender's **Shading** workspace.

Your Sollumz shader exposes texture parameters used by GTA V.
Common examples include:
* diffuse / colour
* normal
* specular
* detail textures

Assign your source images to the appropriate shader parameters.

---

## Embedded Textures
Sollumz allows textures to be marked as **Embedded**.
An embedded texture is stored inside the Drawable rather than being loaded from a separate YTD texture dictionary.
For a small beginner asset this is extremely convenient.

If your prop only uses a couple of unique textures, embedding them can keep everything self-contained.
For larger projects, repeated assets or full MLOs, external **YTD texture dictionaries** become much more useful.
That workflow is covered properly in the full guide.

---

# Texture Basics
You should still treat textures as game assets rather than arbitrary giant PNGs you found on your desktop.
Useful habits include:
* sensible texture resolutions
* power-of-two dimensions where appropriate
* mipmaps
* correct compression
* reusing materials where possible
* avoiding massive unique textures for tiny objects

A technically working asset can still perform terribly.
FiveM does eventually notice when your decorative coffee mug has the texture budget of a small country.

---

# Adding Collision
Right now your model can look perfect while players walk directly through it.
Visual geometry and collision geometry are different jobs.

Your visible mesh answers:
**What does this object look like?**

Collision answers:
**What can actually hit it?**

---

## Create a Collision Mesh
A simple way to start is to duplicate your visual mesh:
`Shift + D`

Then simplify the duplicate.
Remove details that don't affect physical interaction.

Players don't need collision around every screw, groove, bevel and tiny decorative edge.
For a building wall, collision can often be dramatically simpler than the rendered mesh.
Give it a clear name such as:
`ajd_test_wall_col`

---

# Convert the Collision to Bounds
Select your simplified collision geometry.
Use Sollumz's collision / bounds tools to convert it into the appropriate Bound structure.

For an embedded Drawable collision you'll commonly be working toward a hierarchy involving a **Bound Composite** and its child bounds.
Assign an appropriate GTA collision material.

Examples include materials representing:
* concrete
* metal
* wood
* glass

Collision materials aren't just visual labels.
They can influence physical behaviour, impact effects and sounds.

---

# Embed the Collision
If the collision belongs to the Drawable itself, place the collision hierarchy under the appropriate Drawable structure.
The goal is for the exported YDR to contain both:

* visible geometry
* collision

This is especially useful for self-contained props and structures.
Before exporting, inspect the visual model and collision together.

Look for:
* collision floating away from the mesh
* invisible ledges
* missing floors
* overly detailed bounds
* gaps players could fall through
* collision extending through doorways

Collision problems are much easier to fix while you're still looking at them in Blender.

---

# Exporting in Sollumz 2.9
This is one of the places where older tutorials can immediately send you in the wrong direction.

In Sollumz 2.9, use:
**File > Export > Export RAGE Assets**

You don't need to choose something like:
`Export YDR`

Sollumz looks at the asset roots in your scene and determines what those assets are.
Select your Drawable root and open the exporter.
There are three decisions you should understand.

---

## 1. Format

### Native
Creates supported game-ready binary files.

For our Drawable:
`.ydr`
If PyMateria is installed and the format is supported, this is usually the shortest route into FiveM.

### CW XML
Exports CodeWalker-compatible XML.

This remains useful for:
* debugging
* inspection
* unsupported native formats
* platforms without PyMateria
* CodeWalker-based workflows

You can also export both where appropriate.

---

## 2. Game Version
Choose the generation your server actually runs.

### Gen8
FiveM for **GTA V Legacy**

### Gen9
FiveM for **GTA V Enhanced**

If you're creating a public release, you can export both generations.
Keep them separated.
Do not casually throw Gen8 and Gen9 builds into the same release folder and hope FiveM develops intuition.

---

## 3. Scope
While working on one asset, exporting **Selected** is usually the easiest option.

For larger controlled builds you may eventually use **All**.
Make sure the relevant Objects export group is enabled.

For more advanced projects the exporter can also contain groups for things such as:
* YTYP
* YMAP
* Texture Dictionaries

Those become important once your project grows beyond a single Drawable.

---

# Read the Info Log
After exporting, check Sollumz's **Info Log**.
This matters.

There is a difference between:

**the exporter finished**
and
**the exporter exported everything you thought it did**

Warnings can identify:
* missing data
* skipped objects
* invalid hierarchy
* missing map assignments
* material problems
* unsupported data

Don't ignore the first useful error and then spend three hours rebuilding your mesh.
We've all done it.
We don't need to keep doing it.

---

# Testing the Export
For a native export you should now have a `.ydr`.
Depending on your chosen target you'll have exported a:
```text
Gen8 build
```
or:
```text
Gen9 build
```

or both.

A useful development habit is to export into a clean staging folder first rather than directly into your live FiveM resource.
Something like:
```text
exports/
├── gen8/
└── gen9/
```

Then copy the version you're actually testing into your resource.
This prevents files from three experiments ago quietly surviving in your stream folder and making you think your new export works.

---

# Basic FiveM Resource Structure
A simple streamed resource might look like:
```text
my_asset/
├── fxmanifest.lua
└── stream/
    └── my_asset.ydr
```

A minimal manifest can start with:
```lua
fx_version 'cerulean'
game 'gta5'

author 'YourName'
description 'Custom GTA V asset'
version '1.0.0'
```

Files inside the `stream` directory are streamed automatically where appropriate.
However, there is an important distinction:

**streaming a model and placing a model in the world are not the same thing.**
A new custom asset generally also needs the appropriate archetype / placement workflow depending on how you're using it.

For world mapping this usually leads into:
* YTYP archetypes
* YMAP entities
* map containers
* world coordinates
* MLO instances

Sollumz 2.9 now handles far more of this directly inside Blender.
That is where this beginner guide intentionally stops.

---

# So What Have We Actually Built?
At this point you should understand the foundation of almost every larger GTA V asset workflow:

```text
Blender Mesh
    ↓
Sollumz Drawable
    ↓
GTA Shader Material
    ↓
Textures
    ↓
Collision Bounds
    ↓
Export RAGE Assets
    ↓
Native Gen8 / Gen9 YDR
```

Once that makes sense, larger systems stop feeling like random magic.
An MLO isn't just "a Blender interior."

It's a collection of these same asset concepts connected to:
* archetypes
* rooms
* portals
* entities
* doors
* collisions
* lighting
* maps
* streaming data

Learn the layers instead of memorizing button sequences.
You'll debug things significantly faster.

---

# Where the Full Guide Continues

This free guide intentionally gets you through the foundation.
The complete guide goes through the entire production pipeline.

## Full MLO Creation
Build interiors properly with:

* MLO archetypes
* rooms
* portals
* limbo
* entities
* entity sets
* collision ownership
* occlusion
* door entities
* door extensions
* door physics

## FiveM Enhanced
Detailed Gen8 / Gen9 workflows for:

* GTA V Legacy
* GTA V Enhanced
* dual-version releases
* correct export targets

## YMAPs and World Placement
Sollumz 2.9 completely rebuilt its Maps system.

The full guide covers:
* map groups
* containers
* entities
* MLO instances
* automatic partitioning
* world placement
* car generators
* timecycle modifiers
* occluders
* grass batches
* LOD lights

## Native YTD Workflows
Create and manage texture dictionaries directly in Blender:
* shared textures
* YTD imports
* YTD exports
* texture sources
* `+hi` variants
* embedded vs external textures

## GTA V Asset Libraries
Build searchable Blender asset libraries from GTA V assets so you can browse reference assets without repeating the old:
```text
find asset
→ extract asset
→ export XML
→ find textures
→ import
→ repeat
```

workflow every time you need a chair.

## Vehicles
Including:
* Fragment hierarchy
* vehicle shaders
* collisions
* rigging
* windows
* shattermaps
* window previews
* external skeleton workflows

## Production Workflow
Including:
* presets
* staging builds
* reusable asset settings
* optimization
* resource structure
* commercial packaging
* Cfx Asset Escrow
* troubleshooting

---

# Get the Complete Guide
If this guide got you through your first asset, the full guide takes that same approach through the rest of the pipeline.

**Sollumz 2.9.0. Blender 5.2. GTA V Legacy. GTA V Enhanced.**
No guessing which three-year-old tutorial still applies.
No subscription.

Keep the guide and use it as a reference while you build.
### [Get the Complete Sollumz Guide →](https://ajthe.dev/SollumzGuide)

---

# Useful Resources
### Sollumz
https://github.com/Sollumz/Sollumz

### Sollumz Documentation
https://docs.sollumz.org/

### FiveM Documentation
https://docs.fivem.net/

### Cfx.re Forums
https://forum.cfx.re/

### AJTheDev
https://ajthe.dev/

---

# License
This guide is provided for educational use.
Feel free to reference it and link people to the repository.
Please don't redistribute the guide as your own work or repackage it for sale.

---

**Written by [AJTheDev](https://ajthe.dev/)**
North London developer. FiveM, AI, web stuff and an unreasonable amount of time spent finding out why something exported successfully but didn't actually export.
No bullshit.

---

## Star History
If this guide saved you several hours of arguing with Blender, consider starring the repo.

[![Star History Chart](https://api.star-history.com/svg?repos=AlexJamesDean/sollumz-fivem-guide\&type=Date)](https://star-history.com/#AlexJamesDean/sollumz-fivem-guide&Date)
