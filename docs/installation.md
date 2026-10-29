# Installation

## Requirements

- [ROOT](https://root.cern/) — developed and tested against ROOT 6.40.02
- A C++ compiler compatible with your ROOT installation (ShapeIt 2.0 is
  compiled on the fly by ROOT's JIT, so no separate build step is needed)
- A modern web browser (Chrome, Firefox, or Safari) — the user interface
  opens automatically when ShapeIt starts

!!! note "ROOT version"
    ShapeIt 2.0 uses ROOT's `RWebWindow` infrastructure (introduced in
    ROOT 6.12) to serve the browser interface, and `TWebCanvas::ForceUpdate()`
    for non-blocking canvas updates. It has been developed and tested against
    **ROOT 6.40.02**; compatibility with older ROOT versions is not guaranteed.

!!! note
    ShapeIt 2.0 is distributed under the GNU General Public License, version 3
    or later. Bug reports and contributions are welcome via the
    [issue tracker](https://github.com/dennismuecher/ShapeIt2.0/issues).

## Getting the code

```bash
git clone https://github.com/dennismuecher/ShapeIt2.0.git
cd ShapeIt2.0
```

## Launching ShapeIt 2.0

ShapeIt 2.0 is launched as a ROOT macro from the `ShapeIt2.0/` source
directory:

```bash
cd ShapeIt2.0
root WebShapeIt.cxx
```

ROOT compiles the backend, starts a local web server, and opens the ShapeIt
interface in your default browser automatically. The terminal window must
remain open while ShapeIt is running — closing it stops the server.

### Loading a settings file at startup

You can pass a settings file on the command line so ShapeIt loads it
automatically on start:

```bash
root WebShapeIt.cxx -- --settings path/to/mysettings.dat
```

Both `--settings=path/to/mysettings.dat` and `--settings path/to/mysettings.dat`
are accepted. Alternatively, set the environment variable
`SHAPEIT_SETTINGS` before launching:

```bash
export SHAPEIT_SETTINGS=path/to/mysettings.dat
root WebShapeIt.cxx
```

## Upgrading from ShapeIt 1.0

Settings files created by ShapeIt 1.0 can be opened directly in ShapeIt 2.0.
ShapeIt detects the older format automatically and fills in the new fields
with sensible defaults:

- **Peak positions** — estimated as the midpoint of each level's fit window.
  Check and adjust these in the **Levels** tab before running.
- **Active peak for gSF** — defaults to Peak 1 for both levels.
- **Doublet / triplet peaks** — disabled by default.

Once you save the file (**Settings → Save settings**), it is written in
ShapeIt 2.0 format with all new fields included. The original 1.0 file is
overwritten at its path; use **Save settings as...** if you want to keep
the original.

Continue to [Input data format](data-format.md) to prepare your ROOT matrix,
or jump straight to the [user guide](user-guide/loading-a-matrix.md).
