# API reference

ShapeIt 2.0 is organized as a set of ROOT-interpreted C++ classes, all
under `ShapeIt2.0/` (the source directory):

| Class / file | File | Role |
|---|---|---|
| `WebShapeIt` | `WebShapeIt.cxx` | Main entry point: starts the ROOT web server, handles all WebSocket messages from the browser, owns the canvas, drives the display |
| `ShapeSetting` | `ShapeSetting.C` | Holds and (de)serializes all analysis settings; supports reading ShapeIt 1.0 format files and upgrading them to 2.0 |
| `ShapeMatrix` | `ShapeMatrix.C` | Loads the ROOT file, holds the input `TH2`, builds the diagonalized Ex-binned projections |
| `ShapeFitFunction` | `ShapeFitFunction.C` | The Gaussian(s)-on-quadratic-background peak fit function; supports single, doublet, and triplet configurations |
| `ShapeCollector` | `ShapeCollector.C` | Drives extraction across all excitation-energy bins for one run |
| `ShapeGSF` | `ShapeGSF.C` | Builds the Level 1 / Level 2 gSF graphs and performs the symmetric sewing/merge |
| `ShapeRho` | `ShapeRho.C` | Level-density (ρ) extraction and display |
| `ShapeAlpha` | `ShapeAlpha.C` | Monte Carlo slope-correction search: randomizes bin width, Ex lower edge, peak areas, and comparison data to determine $\delta\alpha$ and its 1σ uncertainty |
| `ShapeController` | `ShapeController.C` | Coordinates the analysis pipeline between the web frontend and the analysis classes |

## Communication protocol

ShapeIt 2.0 uses ROOT's `RWebWindow` infrastructure to serve the browser
frontend via WebSocket. Messages follow a keyword-payload pattern.

### Frontend → Backend

| Message | Meaning |
|---|---|
| `RUN` | Run the full extraction with current settings |
| `OPEN:<path>` | Load a ROOT matrix file |
| `SAVE_SETTINGS:<path>` | Save settings to file |
| `LOAD_SETTINGS:<path>` | Load settings from file |
| `SHOWMATRIX` | Display the input matrix |
| `SHOWPROJ` | Display the summed diagonal projection |
| `SHOWBIN:<n>` | Display the projection for bin `n` |
| `SHOW_GSF_RESULTS` | Show the gSF results table |
| `SHOW_LEVEL_DENSITY` | Show the level density plot |
| `SHOW_WIDTH_CALIB` | Show the width calibration plot |
| `LEVELS:<lo1\|hi1\|lo2\|hi2\|exlo\|exhi>` | Update level windows and excitation range |
| `ALPHA_TRANSFORM:<alpha\|lit_norm>` | Apply slope transformation |
| `RUN_MC:<iterations\|exiMin\|exiMax>` | Start Monte Carlo α search |
| `STOP_MC` | Interrupt a running Monte Carlo |
| `PEAK_CONFIG:<...>` | Update peak configuration (mode, doublet/triplet, width fix) |
| `PEAK_POS1:<...>` | Update Level 1 peak positions |
| `PEAK_POS2:<...>` | Update Level 2 peak positions |
| `LOAD_OSLO:<path>` | Load Oslo gSF literature file |
| `LOAD_RHO:<path>` | Load NLD literature file |
| `LOAD_DISCRETE:<path>` | Load discrete level scheme file |
| `LOAD_EFFI:<path>` | Load energy-dependent efficiency file |

### Backend → Frontend

| Message | Meaning |
|---|---|
| `SETTINGS_SYNC:<pipe-delimited values>` | Full settings state pushed to the browser after loading a file |
| `DIRLIST:<path\|entries>` | Directory listing for the file browser |
| `MATRIXLIST:<index\|names>` | Available TH2 histogram names in the loaded file |
| `NBINS:<size0\|size1\|nLo\|nHi>` | Recomputed bin counts after excitation range or bin size changes |
| `GSF_RESULTS:<data>` | Tabular gSF results |
| `WIDTH_CALIB_AVAILABLE:<0\|1>` | Whether width calibration data exists |
| `WIDTH_CALIB_PARAMS:<...>` | Width calibration fit parameters |
| `MC_STARTED` | Monte Carlo simulation has started |
| `MC_STOPPING` | Stop requested, waiting for current iteration |
| `MC_STOPPED:<reason>` | Monte Carlo has stopped (completed or interrupted) |
| `MC_RESULT:<alpha\|chi2\|exLow\|iter\|total>` | Per-iteration result during the Monte Carlo |
| `UNIT_WARNING_MeV:<maxEnergy>` | Loaded Oslo file energies appear to be in MeV |
| *any other string* | Logged as a status message in the log panel |

!!! info "Auto-generated reference"
    This table is a manually curated map of the codebase. A fully
    auto-generated, function-level API reference (e.g. via Doxygen) could
    complement this narrative guide.
