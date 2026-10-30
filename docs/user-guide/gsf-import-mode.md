# Importing an external γSF

ShapeIt 2.0 can work with an externally extracted γSF — for example, one
obtained from a different code or from the Oslo method — without requiring a
raw matrix at all. This **import mode** lets you apply the same slope
correction and Monte Carlo uncertainty workflow to any γSF in a plain text
file.

## Entering import mode

From the menu bar, choose **File → Import gSF data...** The standard file
browser opens. Navigate to your text file and click **Open**.

As soon as a valid file is selected:

- The canvas displays the imported γSF points on a logarithmic y-axis.
- A yellow banner — **Import mode — no matrix loaded** — appears at the top of
  the sidebar.
- The **ShapeIt!** run button changes to **Re-read gSF files**.
- The **Levels**, **Projections**, and **Options** sidebar tabs are dimmed and
  cannot be selected (they have no meaning without a matrix).
- Several fields in the **Excitation & Bins** tab and the **Monte Carlo Alpha
  Search** section are greyed out for the same reason.

## File format

The import file must be a plain text file with **three whitespace-separated
columns per line**:

```
# Optional comment lines starting with '#' are ignored
Egamma_keV   gSF   gSF_error
```

| Column | Content |
|---|---|
| 1 | γ-ray energy (keV) |
| 2 | γSF value (MeV⁻³) |
| 3 | Statistical uncertainty on the γSF (MeV⁻³) |

Lines beginning with `#` are treated as comments and skipped. Lines that
cannot be parsed as three numbers are also skipped silently.

!!! tip
    The energy column is in **keV**, consistent with the rest of ShapeIt.
    If your file uses MeV, multiply the energy column by 1000 before
    importing. The same MeV-detection warning that applies to Oslo literature
    files does **not** apply here — the values are read as-is.

## Comparing with Oslo literature data

Once in import mode, you can still load an Oslo-method γSF for comparison:

**Settings → Load gSF literature (Oslo) data...**

The Oslo curve is overlaid in blue (hatched band) on the same canvas as the
imported points. A χ²/n label is computed from the overlap region and
displayed directly on the plot, so you can judge agreement at a glance.

The same MeV auto-conversion check applies here as in normal mode — see
[Monte Carlo uncertainty and absolute level density](monte-carlo-uncertainty.md#load-the-comparison-oslo-method-data).

## Slope correction in import mode

**Transformation → Slope correction...** opens the **Slope Correction** panel
exactly as in normal mode. You can:

- Adjust **Slope Δα** and **gSF scale B** manually and see the overlaid
  literature curve update live.
- Run the **Monte Carlo Alpha Search** to determine Δα and its uncertainty
  quantitatively.

### Monte Carlo in import mode

The Monte Carlo algorithm adapts to the absence of a matrix:

- **There is no excitation-energy range to vary.** The **Ex low min** and
  **Ex low max** fields are greyed out — they apply only when a matrix is
  loaded and the excitation range can be randomised.
- On each iteration, ShapeIt instead **perturbs the imported gSF values** by
  Gaussian noise (centred on each point's value, width = its quoted error),
  and simultaneously **perturbs the Oslo literature points** by their
  uncertainties.
- The best-fit Δα is found by χ² minimisation against the (resampled)
  literature curve, over all import points that overlap the Oslo energy range.
- The four-panel summary (χ² vs Δα, Δα histogram, etc.) is produced on
  completion, exactly as in normal mode.

The **Iterations** field works identically to normal mode.

## Re-reading the file

If you update the file on disk (e.g. after re-running your extraction code),
click **Re-read gSF files** (the renamed ShapeIt! button) to reload and
redisplay the data without reopening the file browser.

## Exiting import mode

Import mode is exited automatically when you open a matrix via
**File → Open matrix...**  The banner disappears, the sidebar tabs are
restored, and the interface returns to normal ShapeIt workflow.
