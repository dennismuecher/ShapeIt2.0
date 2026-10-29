# 3. Choosing mode and binning

All binning and scaling settings live in the **Excitation & Bins** tab.
The mode (Integration vs. Autofit) is set in the **Options** tab.

## Mode: Integration vs Autofit

In the **Options** tab, under **Mode**, choose how each peak's intensity is
extracted in every excitation-energy bin:

- **Integration** (default) — sums the raw counts in the peak fit region
  (a simple rectangular sum over the bins).
- **Autofit** — fits each peak with a Gaussian on a background (see
  [The algorithm](../algorithm.md#peak-fitting) for the functional form),
  useful when peaks sit on a steeply varying background or overlap with
  nearby contaminant lines. The fitted Gaussian's integral is used as the
  peak's intensity, in place of the raw sum.

### How background subtraction works in each mode

When **Background subtraction** is enabled (in the **Options** tab), ShapeIt
performs the Autofit-style Gaussian-on-background fit internally to obtain a
background estimate under each peak, *regardless* of which Mode is selected.
What differs between the two modes is how the **signal** is treated:

- In **Autofit** mode, the peak's reported intensity is the fitted
  Gaussian's integral, with the fitted background's integral subtracted.
- In **Integration** mode, the peak's reported intensity is the raw
  rectangular sum over the peak window, rescaled by the fraction of it that
  the background fit identifies as signal rather than background.

Both modes therefore use the same underlying background fit when background
subtraction is on. If **Background subtraction** is off, Integration mode
uses the raw rectangular sum with no correction.

## Binning the excitation-energy axis

In the **Excitation & Bins** tab:

| Field | Meaning |
|---|---|
| **Excitation** (low – high) | The Ex range over which to step through excitation-energy bins |
| **Bin size [keV]** | Width of each excitation-energy slice |
| **Nr. of bins** | Number of slices to analyze (computed automatically from the excitation range and bin size) |
| **Min. counts** | Minimum peak counts required to accept a bin — see below |
| **gSF scaling / Auto** | Overall normalization of the extracted γSF — see below |
| **Efficiency correction** | Correction factor applied to Level 2's intensity — see below |

Enabling **Bin size variation** (checkbox next to the Bin size field)
activates the upper bound of the bin size range and lets ShapeIt repeat the
extraction over a range of bin sizes as a systematic-uncertainty check. See
[Options and sewing](options-and-sewing.md) for more detail.

## Min. Counts

If either peak's intensity in a bin falls below this threshold, the entire
bin is skipped — it doesn't contribute a γSF pair. This protects the
strength ratio (see [The algorithm](../algorithm.md)) against bins where a
peak isn't statistically significant above background, which would otherwise
produce a wildly unreliable ratio.

## gSF scaling and Auto

The **gSF scaling** field sets an overall multiplicative normalization
applied to your final extracted γSF curve — the *shape* of the sewn curve is
fixed by the sewing algorithm, but its absolute vertical placement on the
plot is still free until you set this.

- **Auto checked** (and literature γSF data loaded via
  **Settings → Load gSF literature (Oslo) data...**): ShapeIt determines
  this factor automatically, by a least-squares fit of your extracted γSF
  onto the literature curve over their overlapping energy range. The field
  becomes read-only and shows the resulting value.
- **Auto unchecked**, or no literature data loaded: you set the value by
  hand. Internally, without a literature anchor, ShapeIt instead normalizes
  multiple sliding-window/bin-size realizations to each other (using the
  very first realization as the reference) so they overlay consistently.

## Eff. Corr.

Level 1 and Level 2 aren't always detected with the same relative
efficiency — for example if they have different spins, or due to detector
geometry effects. ShapeIt 2.0 has two independent ways to correct for this,
which combine when both are used:

- **Efficiency correction** (the number field in the **Excitation & Bins**
  tab) is a single, **energy-independent** factor that always multiplies
  **Level 2's** intensity before the ratio $R$ is formed.
- **Settings → Load eff-corr file...** loads an optional,
  **energy-dependent** efficiency curve from a file, interpolated and
  applied to *both* levels at their respective γ-ray energies (falling back
  to a factor of 1 outside the energies covered by the file). Once loaded,
  the checkbox **Use energy-dependent file** (below the scalar field)
  becomes active.

The recommended way to determine the scalar efficiency correction value:
simulate your experiment (e.g. with GEANT4) using a known input γSF, run
that simulated data through the same Shape-method analysis, and adjust the
correction until ShapeIt's extracted γSF reproduces the input you simulated
with.

!!! tip "Efficiency correction file format"
    See [Settings files](../settings-files.md#load-eff-corr-file) for
    the exact two-column format expected by the energy-dependent efficiency
    file.

Next: [Options and sewing](options-and-sewing.md).
