# Stellar Spectral Analysis

A Python notebook for analysing stellar spectra using Fe I absorption lines — including excitation temperature estimation via the multiplet method, equivalent width measurement, and chi-squared fitting against the BT-NextGen synthetic spectrum grid.

Originally developed as a computational astrophysics assignment, shared openly for students and researchers working with stellar spectroscopy.

---

## What this does

- Loads 1D wavelength-calibrated spectra from FITS files
- Groups Fe I lines into multiplets and fits curves of growth to derive excitation temperatures via the Boltzmann equation
- Measures equivalent widths by Gaussian fitting and compares them against a synthetic model grid (BT-NextGen)
- Applies instrumental resolution and rotational broadening to synthetic spectra
- Performs Fourier analysis of line profiles to estimate v sin i

---

## Repository structure

```
stellar-spectral-analysis/
│
├── stellar_spectral_analysis.ipynb   # Main analysis notebook
│
├── data/
│   ├── estrela1.fits                 # Observed spectrum — Star 1
│   ├── estrela2.fits                 # Observed spectrum — Star 2
│   ├── Imagem_Espectral.fits         # 2D spectral image
│   ├── Espetro_Sintético.dat         # Synthetic reference spectrum
│   ├── line_list_tsantaki.dat        # Full Fe I+II line list (Tsantaki et al.)
│   ├── line_list_tsantakiFEI.dat     # Fe I lines only
│   ├── line_list_tsantakiFEII.dat    # Fe II lines only
│   ├── espetros 5000-6000/           # BT-NextGen grid — NOT included (see below)
│   └── espetros 6000-7000/           # BT-NextGen grid — NOT included (see below)
│
├── results/                          # Output plots saved here by the notebook
├── requirements.txt
├── .gitignore
└── LICENSE
```

### BT-NextGen synthetic spectra (not included)

The `espetros 5000-6000/` and `espetros 6000-7000/` folders contain BT-NextGen model spectra and are excluded from this repository due to size (> 1 GB). To run the spectral fitting section you need to download them manually:

1. Go to the [PHOENIX model grid](https://phoenix.ens-lyon.fr/Grids/BT-NextGen/)
2. Download the T_eff ranges you need (5000–6000 K and 6000–7000 K)
3. Place the files in `data/espetros 5000-6000/` and `data/espetros 6000-7000/`

The sections on multiplet analysis, equivalent width measurement, and spectral visualisation work without the grid — only the chi-squared grid search requires it.

---

## Requirements

- Python 3.8+
- astropy
- numpy
- matplotlib
- scipy
- jupyter

Install everything with:

```bash
pip install -r requirements.txt
```

---

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/stellar-spectral-analysis.git
cd stellar-spectral-analysis
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. (Optional) Download the BT-NextGen grid

See the section above. Skip this if you only want to run the multiplet and EW analysis.

### 4. Run the notebook

```bash
jupyter notebook stellar_spectral_analysis.ipynb
```

All included data files are loaded via relative paths from `data/` — no configuration needed.

---

## Data files

| File | Description |
|---|---|
| `estrela1.fits` | 1D spectrum of Star 1 (wavelength-calibrated, CRVAL1/CDELT1 WCS) |
| `estrela2.fits` | 1D spectrum of Star 2 (shorter wavelength range, ~5855–6840 Å) |
| `Imagem_Espectral.fits` | Raw 2D spectral image |
| `Espetro_Sintético.dat` | Two-column synthetic spectrum (wavelength Å, flux) |
| `line_list_tsantaki.dat` | Combined Fe I + Fe II line list |
| `line_list_tsantakiFEI.dat` | Fe I lines: wavelength (Å), EP (eV), log gf, EW☉ (mÅ) |
| `line_list_tsantakiFEII.dat` | Fe II lines (same format) |

---

## Scientific background

This workflow implements the **excitation equilibrium method** for stellar parameter determination:

1. Fe I lines are grouped into multiplets by excitation potential (EP)
2. For each multiplet the reduced equivalent width W_λ/λ is plotted against log(λ·gf) — the curve of growth
3. The horizontal shift between two multiplets at different EPs gives the excitation temperature via the Boltzmann equation: T = 5040 × ΔEP / Δ
4. The observed EWs are then compared against those of BT-NextGen synthetic spectra to estimate T_eff, log g, and [Fe/H]
5. For Star 2, rotational broadening (v sin i ≈ 10.8 km/s) is applied before comparison

**Reference:** Tsantaki et al. (2013), *A&A*, 555, A150 — [doi:10.1051/0004-6361/201321103](https://doi.org/10.1051/0004-6361/201321103)

---

## License

MIT — see [LICENSE](LICENSE) for details.
