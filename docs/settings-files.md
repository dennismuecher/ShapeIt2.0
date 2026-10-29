# Settings files

ShapeIt 2.0 can save your entire analysis configuration (matrix, energy
windows, mode, options, calibrations) to a plain-text settings file, and
reload it later — useful for reproducing an analysis or sharing a
configuration with a collaborator.

## Saving and loading

Under the **Settings** menu:

- **Open settings...** — open a previously saved `.dat` settings file
- **Save settings** — overwrite the currently loaded settings file
- **Save settings as...** — save to a new file

The current settings filename is shown in the menu bar, with a status
indicator: an asterisk (*) means there are unsaved changes.

!!! tip "Viewing the current settings"
    **Display → Show settings file...** dumps the current, active settings
    as they would appear in a file — handy for a quick sanity check without
    saving.

## Upgrading ShapeIt 1.0 settings files

ShapeIt 2.0 can open settings files created by ShapeIt 1.0. The first time
you open a 1.0 file, a dialog offers to upgrade it to the 2.0 format
(which adds the new fields for peak positions, triplet peaks, display
options, etc.). The original file is left unchanged; a new file is saved at
the path you choose.

## File format

The settings file is a simple whitespace-separated text format, one entry
per line: a keyword followed by its value(s). The main keys are:

| Key | Meaning |
|---|---|
| `MeV:` | Intended unit flag (1 = keV, 1000 = MeV) — **unreliable, see the [units warning](data-format.md); convert your matrix to keV instead** |
| `mode` | Integration (1) or Autofit (2) |
| `level1`, `level2` | Window (low, high) on the Ex − Eγ axis bracketing final level $L_1$ / $L_2$ |
| `bg_level1`, `bg_level2` | Four background-window edges (same Ex − Eγ axis) for each level |
| `excitation` | Ex range (low, high) analyzed |
| `excitationMCLimits` | Range for the randomized lower Ex edge in the Monte Carlo procedure |
| `excitation_bin_1`, `excitation_bin_2` | Bin size(s) [keV] |
| `nOfBins` | Number of excitation-energy bins |
| `minCounts` | Minimum peak counts to accept a bin |
| `eff_corr` | Efficiency-correction factor |
| `alphaLimit` | Trial slope-correction ($\alpha$) range for the Monte Carlo simulation |
| `alphaIter` | Number of steps in the χ² search over that $\alpha$ range |
| `doInterpol` | Sewing interpolation on/off |
| `doBackground` | Background subtraction on/off |
| `doSlidingWindow` | Sliding-window variation on/off |
| `doBinVariation` | Bin-size variation on/off |
| `doWidthCal` | Use width calibration on/off |
| `doOslo` | Overlay literature/expectation values on/off |
| `doAutoScale` | Automatic gSF normalization on/off |
| `gSF_norm`, `lit_norm` | Normalization values |
| `widthCal` | Stored peak-width calibration parameters (offset and slope for Level 1 and Level 2) |
| `rhoFileName`, `rhoScale` | Level-density comparison file and scale |
| `discreteLevelFile`, `discreteBins` | Discrete level scheme file, for the level-density panel |
| `peakPos` | Peak 1 positions for Level 1 and Level 2 (keV) |
| `doubletPeakPos` | Peak 2 positions for Level 1 and Level 2 (keV) |
| `tripletPeakPos` | Peak 3 positions for Level 1 and Level 2 (keV) |
| `fixPeakPos` | Whether Peak 1 position is fixed (0/1) for each level |
| `fixDoubletPeakPos` | Whether Peak 2 position is fixed (0/1) for each level |
| `fixTripletPeakPos` | Whether Peak 3 position is fixed (0/1) for each level |
| `doDoublet` | Peak 2 enabled (0/1) for each level |
| `doTriplet` | Peak 3 enabled (0/1) for each level |
| `gsfPeakRef` | Which peak (1/2/3) is used for gSF for each level |
| `displaySingle` | Show individual data points on/off |
| `displayAvg` | Show average values on/off |
| `colour` | Different colors for peaks on/off |
| `verbose` | Verbose output on/off |

!!! tip
    You don't need to hand-edit these files — **Save settings as...** writes
    a complete, valid file from your current interface state. Hand-editing
    is mainly useful for the few settings with no direct interface control,
    or for scripting a batch of related analyses.

!!! warning "Reload after hand-editing"
    ShapeIt reads the settings file only when you open it — it does **not**
    watch the file for background changes. If you edit the file by hand while
    ShapeIt is open, the change has no effect until you re-read it with
    **Settings → Open settings...**.

## Related data files

Under **Settings**, you can separately load four kinds of external data
file. Each is a plain-text file with one data point per line and
whitespace-separated columns — but the **column meaning differs for each**,
which is a common source of confusion. The exact formats are below.

### Load gSF literature (Oslo) data...

The comparison γSF (e.g. from an Oslo-method analysis of the same data),
overlaid when **Display expectation** is enabled and used as the reference
for the slope determination.

Three columns per line:

```
E_gamma    gSF_high    gSF_low
```

| Column | Meaning |
|---|---|
| `E_gamma` | γ-ray energy, in keV |
| `gSF_high` | Upper edge of the γSF value (value + its uncertainty) |
| `gSF_low` | Lower edge of the γSF value (value − its uncertainty) |

!!! warning "This file gives an error band, not value + error"
    ShapeIt does **not** read a central value and a symmetric error. It reads
    the **high and low edges** of each point, then internally takes the
    central value as $(\text{high}+\text{low})/2$ and the 1σ uncertainty as
    $(\text{high}-\text{low})/2$. If you have value ± error instead, convert
    to `value+error` and `value−error` before saving.

!!! note "Automatic MeV → keV conversion"
    When loading an Oslo gSF file, ShapeIt checks whether the energies look
    like MeV (all values below a threshold). If so, a warning dialog offers
    to convert them to keV automatically.

### Load NLD literature (Oslo) data...

The comparison level density (e.g. an Oslo-method ρ for the same nucleus),
overlaid on the level-density panel.

Three columns per line:

```
energy    rho    rho_error
```

| Column | Meaning |
|---|---|
| `energy` | Excitation energy, in **MeV** |
| `rho` | Level density, in MeV⁻¹ |
| `rho_error` | Uncertainty on the level density, in MeV⁻¹ |

!!! note
    Unlike the γSF and efficiency files (which use keV), the level-density
    file's energy column is in **MeV**, matching the level-density plot's
    axis.

### Load discrete levels data...

A discrete level scheme to overlay on the level-density plot at low
excitation energy. Two columns per line:

```
energy    spin_parity
```

| Column | Meaning |
|---|---|
| `energy` | Level energy, in keV |
| `spin_parity` | Spin-parity label (e.g. `2+`) — used only for display |

### Load eff-corr file...

The energy-dependent efficiency correction — the energy-dependent
counterpart to the scalar **Efficiency correction** field. When loaded,
it is interpolated and applied to *both* levels at their respective γ-ray
energies (returning a factor of 1 outside the energy range covered by the
file), and it combines with the scalar efficiency correction (which always
additionally multiplies Level 2).

Two columns per line:

```
E_gamma    factor
```

| Column | Meaning |
|---|---|
| `E_gamma` | γ-ray energy, in keV |
| `factor` | Multiplicative scaling factor applied to the peak intensity at that energy |

!!! tip "How file paths are stored"
    Once loaded, the paths to these files are saved *inside* your settings
    file, so re-loading a settings file automatically re-loads the associated
    data files — you don't have to re-select them each time.
