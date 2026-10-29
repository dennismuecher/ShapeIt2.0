# Installation

## Requirements

- [ROOT](https://root.cern/) — developed and tested against ROOT 6.34
- A C++ compiler compatible with your ROOT installation (ShapeIt 2.0 is
  compiled on the fly by ROOT's JIT, so no separate build step is needed)
- A modern web browser (Chrome, Firefox, or Safari) — the user interface
  opens automatically when ShapeIt starts

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

Settings files created by ShapeIt 1.0 can be opened directly in ShapeIt 2.0
— the first time you open a 1.0 file, ShapeIt will offer to upgrade it to the
2.0 format, which adds the new fields for peak positions, triplet peaks, and
display options. The original file is left unchanged; a new file is saved at
the path you choose.

Continue to [Input data format](data-format.md) to prepare your ROOT matrix,
or jump straight to the [user guide](user-guide/loading-a-matrix.md).
