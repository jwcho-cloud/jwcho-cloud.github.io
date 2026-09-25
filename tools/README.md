# IR Materials Explorer

Interactive companion to:

> J.-W. Cho, T. Kumar, H. Mei, and M. A. Kats, "Material selection for mid-infrared coatings and windows," arXiv:2605.29079 (2026). In revision, *Laser & Photonics Reviews*.

Live page: <https://jwcho-cloud.github.io/tools/>

## Files

| File | Contents |
|---|---|
| `index.html` | Page layout, controls, plotting and hover logic |
| `ir-materials-data.js` | Optical constants `MAT` (n, k vs. wavelength) and material groups/colors `GROUPS` |
| `README.md` | This document: method, display rules and change log |

The data file is a verbatim copy of the `MAT` and `GROUPS` blocks from the original single-file tool (`IR_transparency_window_figure_plot.html`). No values were changed.

## Materials (25)

| Group | Materials |
|---|---|
| Fluoride insulators | poly-MgF₂, poly-CaF₂, poly-BaF₂ |
| Nitride insulators | a-SiN, a-AlN |
| Oxide insulators | a-SiO₂, a-Al₂O₃, poly-Y₂O₃, a-HfO₂, a-TiO₂ |
| III-V semiconductors | c-GaP, c-GaAs |
| IV semiconductors | a-Si, c-Si, a-Ge, c-Ge |
| Chalcogenide semiconductors | poly-ZnS, poly-ZnSe, c-CdTe |
| Complex oxide/nitride insulators | poly-ALON, poly-MgAl₂O₄ (spinel) |
| Halide insulators | c-KBr, c-NaCl, c-KRS-5 |
| Diamond | c-Diamond |

The source reference for each material is stored in `ir-materials-data.js` (`source` / `sources`) and is written into the header of every CSV download.

## Definitions

- Absorption coefficient: **α = 4πκ / λ**, reported in cm⁻¹ (λ in μm converted to m, then m⁻¹ → cm⁻¹).
- **Transparency window**: wavelengths where α is below the user-set threshold α<sub>th</sub> (default 10 cm⁻¹; presets 1, 10, 100, 1000 cm⁻¹).
- In the κ view, the same threshold is drawn as **κ<sub>th</sub>(λ) = α<sub>th</sub> λ / 4π**, so it rises linearly with wavelength.

## Plots

### Refractive Index *n* in the Transparency Window (left)

- Plots n(λ) only at points where α < α<sub>th</sub> and λ is inside the chosen wavelength range. Outside the window, the curve is broken.
- Points with n ≤ 0 are skipped.
- The y-range is automatic (data range ±5 %, lower bound ≥ 0.9) unless n min/max are entered.

### Absorption Coefficient *α* / Extinction Coefficient *κ* across the Full Spectrum (right)

- Plots all data in the wavelength range. The full curve is drawn thin and faded, and the part inside the transparency window is drawn bold.
- The threshold is drawn as a dashed green line with the region below it shaded.
- **Reported-k rule:** for materials that list the wavelengths where k was actually reported (`show_k_lams`: c-CdTe, c-GaAs, c-GaP, poly-ZnS, c-KRS-5, c-KBr, c-NaCl), only those points are plotted in this panel.
- **k = 0 rule:** on a log axis, points with κ = 0 (e.g., where a source assumes k = 0) cannot be drawn and are omitted. They are still used for the transparency test (α = 0 < α<sub>th</sub>).
- Default y-axis is log. The range spans at most 14 decades below the maximum and always includes the threshold.

### Shared behavior

- Both plots use the same wavelength range and the same linear/log wavelength axis.
- Hovering (or tapping) a curve in either plot finds the nearest data point within 18 px and then:
  - marks that wavelength in both plots,
  - puts a marker on that material's curve in each plot,
  - shows λ, n, κ and α. α is green when inside the transparency window and red when outside.
- Hovering a material in the list highlights its curves and fades the rest.

### Axis ticks

- Linear axes use 1-2-5 steps (about 6–7 labels), so narrow ranges (e.g., n = 1.30–1.50) still get labels.
- Log axes use decade ticks (with sub-decade ticks when fewer than five decades are shown). On the α/κ panel they are thinned to about 8 labels when many decades are shown.

## CSV download

Per material, via the download icon in the list. The format is unchanged from the original tool:

- Header comment lines: material, source(s) with wavelength ranges, DOI(s), crystallinity, temperature.
- Columns: `wl_um,n,k`.
- The file honors `show_n_lams` / `show_k_lams`: n or k is left blank at wavelengths where it was not reported.

## Change log

### 2026-09-25

- Moved the tool from the single-file version into the website at `/tools/`, with the site's sidebar and navigation.
- Split the data into `ir-materials-data.js` (values unchanged).
- Added the α / κ panel beside the n panel, with the threshold line and a linked hover readout.
- Renamed the plot titles to "Refractive Index *n* in the Transparency Window" and "Absorption Coefficient *α* / Extinction Coefficient *κ* across the Full Spectrum".
- Replaced the original linear tick routine, which used a minimum step of 0.5, with 1-2-5 steps so that axis labels appear on narrow n and λ ranges.
- The α/κ panel plots only reported-k points for materials that define `show_k_lams`. The original n-vs-λ figure ignored `show_n_lams` / `show_k_lams`, and the n panel still does, to match the original figure.
