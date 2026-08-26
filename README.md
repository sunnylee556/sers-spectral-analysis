# sers-spectral-analysis

Python/Jupyter tools for analyzing Surface-Enhanced Raman Spectroscopy (SERS) hyperspectral data. Each notebook takes a Raman data cube (spatial x, spatial y, spectral) stored as TIFF (or `.npz`) and extracts per-pixel maximum intensity information, with optional cosmic-ray removal and spectral-range filtering.

## What's here

| Notebook | Description |
|---|---|
| [find-max-final.ipynb](find-max-final.ipynb) | Baseline analysis: loads a Raman cube from a `.npz` file, computes the maximum intensity (and corresponding Raman shift) per pixel, and visualizes results as an intensity map and histogram. |
| [sers-find-max.ipynb](sers-find-max.ipynb) | Extends the baseline workflow to TIFF input. |
| [sers-filtered-find-max.ipynb](sers-filtered-find-max.ipynb) | Adds cosmic-ray removal before computing per-pixel maximum intensity, so spurious spikes don't skew the results. |
| [find-max-final-1_completed (1).ipynb](find-max-final-1_completed%20(1).ipynb) | Full batch pipeline: loads every TIFF in a folder, removes cosmic rays, isolates a target spectral range (e.g. 1600–3000 cm⁻¹), saves cleaned/filtered cubes to a new folder, and plots example pixel spectra and single-wavelength images. |

## Method overview

1. **Load** a Raman data cube, typically shaped `(nx, nspec, ny)` — two spatial axes and one spectral axis.
2. **Clean cosmic rays**: each spectrum is scanned for narrow, sharp spikes (width ≤ 2 samples at half-max) using `scipy.signal.find_peaks`/`peak_widths`, which are replaced by interpolating neighboring points.
3. **Filter by spectral range** (optional): restrict analysis to a wavenumber window of interest (e.g. 1600–3000 cm⁻¹).
4. **Compute per-pixel maximum intensity** (and the wavenumber at which it occurs) via `np.max`/`np.argmax` along the spectral axis.
5. **Visualize**: intensity heatmaps, histograms of per-pixel maxima, and individual pixel spectra.

## Requirements

- Python 3
- `numpy`
- `scipy`
- `matplotlib`
- `tifffile`
- `pandas`, `scikit-image`, `opencv-python` (`cv2`), `spectrochempy` — used in some exploratory cells

Install with:

```bash
pip install numpy scipy matplotlib tifffile pandas scikit-image opencv-python spectrochempy
```

## Usage

1. Open a notebook in Jupyter or VS Code.
2. Update the data path (`tiff_path`, `folder_path`, or the `.npz` path) to point to your own Raman cube data.
3. If your cube's axis order differs from `(nx, nspec, ny)`, reorder it with `np.moveaxis` as shown in the loading cell.
4. Run all cells. For the batch pipeline, cleaned/filtered TIFFs are written to a `cleaned_1600_3000` subfolder alongside the input data.

## Data

Sample/test data paths in these notebooks are placeholders (or point to local machine paths) and are **not included** in this repository. Point each notebook at your own dataset before running.
