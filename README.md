# TMD Alloys — Electronic and Optical Dataset

Electronic band structures and optical properties of monolayer transition metal
dichalcogenide (TMD) alloys built from tungsten (W), molybdenum (Mo) and the
chalcogens sulfur (S), selenium (Se) and tellurium (Te), obtained from
first-principles density functional theory (DFT) calculations.

The dataset covers the binary end members (MoX₂, WX₂) and the full range of
ternary W–Mo compositions for each chalcogen family.

---

## Repository structure

```
Binary_and_ternary_data/
├── WMoS2_data/
│   ├── Mo4S8_data/
│   ├── W2Mo2S8_data/
│   ├── W2Mo14S32_data/
│   ├── ...
│   └── W4S8_data/
├── WMoSe2_data/
│   └── ...
└── WMoTe2_data/
    └── ...
```

Each material folder contains:

| File | Description |
| --- | --- |
| `<name> Optical Properties x.csv` | Optical response for in-plane (x) polarization |
| `<name> Optical Properties z.csv` | Optical response for out-of-plane (z) polarization |
| `<name>_absorption_vs_wavelength.png` | Absorption spectrum |
| `<name>_absorption_fit_vs_wavelength.png` | Fitted absorption spectrum |
| `<name>_epsilon_vs_wavelength.png` | Real and imaginary dielectric function |
| `<name>_nk_vs_wavelength.png` | Refractive index *n* and extinction coefficient *k* |
| `bands.out.gnu` | Raw band-structure data, two columns: *k* and *E* (eV) |
| `<name>_bandstructure.png` | Band structure plotted along Γ–M–K–Γ |

### Naming convention

Folder names give the atom counts in the simulation cell. For example:

- `Mo4S8` — binary MoS₂ (4 Mo, 8 S)
- `W2Mo2S8` — ternary W₀.₅Mo₀.₅S₂ (2 W, 2 Mo, 8 S)
- `W7Mo9Te32` — ternary W₀.₄₃₈Mo₀.₅₆₂Te₂ (7 W, 9 Mo, 32 Te)

The W:Mo ratio therefore sets the alloy composition *x* in W*ₓ*Mo₁₋*ₓ*X₂.

---

## Quaternary and quinary CASTEP data

The quaternary and quinary datasets were generated using CASTEP within BIOVIA Materials Studio and are distributed in the following archives:

- `WMoSSe_data.rar`
- `WMoSTe_data.rar`
- `WMoSeTe_data.rar`
- `WMoSSeTe_data.rar`

After extraction, each archive contains composition-specific material folders.

### Quaternary alloy datasets

```text
Quaternary_CASTEP_data/
├── WMoSSe_data/
│   ├── Mo3W13S6Se26_data/
│   ├── Mo3W13S13Se19_data/
│   ├── Mo3W13S19Se13_data/
│   ├── Mo3W13S26Se6_data/
│   └── ...
│
├── WMoSTe_data/
│   ├── Mo3W13S6Te26_data/
│   ├── Mo3W13S13Te19_data/
│   ├── Mo3W13S19Te13_data/
│   ├── Mo3W13S26Te6_data/
│   └── ...
│
└── WMoSeTe_data/
    ├── Mo3W13Se6Te26_data/
    ├── Mo3W13Se13Te19_data/
    ├── Mo3W13Se19Te13_data/
    ├── Mo3W13Se26Te6_data/
    └── ...
```

### Quinary alloy dataset

```text
Quinary_CASTEP_data/
└── WMoSSeTe_data/
    ├── Mo3W13S4Se24Te4_data/
    ├── Mo3W13S11Se17Te4_data/
    ├── Mo3W13S17Se11Te4_data/
    └── ...
```
Each CASTEP material folder contains:

| File | Description |
| --- | --- |
| `<name> Band Structure.csv` | Raw CASTEP electronic band-structure data containing the normalized *k*-path coordinate and energy in eV |
| `<name> DOS.csv` | Total electronic density of states |
| `<name> Optical Properties x.csv` | Optical response for in-plane (x) polarization |
| `<name> Optical Properties z.csv` | Optical response for out-of-plane (z) polarization |
| `plots/` | Subdirectory containing the generated electronic and optical figures |

### Generated figures

| File | Description |
| --- | --- |
| `<name>_band_dos.png` | Electronic band structure and total density of states |
| `<name>_dielectric.png` | Real and imaginary dielectric functions for the x and z polarizations |
| `<name>_nk.png` | Refractive index *n* and extinction coefficient *k* |
| `<name>_absorption_vs_wavelength.png` | Absorption coefficient as a function of wavelength |

### Quaternary and quinary naming convention

Folder names specify the number of atoms of each element in the CASTEP simulation cell. For example:

- `Mo3W13S6Se26_data` — 3 Mo, 13 W, 6 S, and 26 Se atoms
- `Mo3W13S26Te6_data` — 3 Mo, 13 W, 26 S, and 6 Te atoms
- `Mo3W13Se19Te13_data` — 3 Mo, 13 W, 19 Se, and 13 Te atoms
- `Mo3W13S4Se24Te4_data` — 3 Mo, 13 W, 4 S, 24 Se, and 4 Te atoms

The elemental fractions can be calculated directly from the corresponding atom counts.

For a quaternary Mo–W–X–Y alloy, the W fraction is

```math
x_{\mathrm{W}} = \frac{N_{\mathrm{W}}}{N_{\mathrm{W}} + N_{\mathrm{Mo}}}
```

and the chalcogen-X fraction is

```math
x_{\mathrm{X}} = \frac{N_{\mathrm{X}}}{N_{\mathrm{X}} + N_{\mathrm{Y}}}
```

For a quinary Mo–W–S–Se–Te alloy, the individual chalcogen fractions are

```math
x_i = \frac{N_i}{N_{\mathrm{S}} + N_{\mathrm{Se}} + N_{\mathrm{Te}}}, \qquad i = \mathrm{S}, \mathrm{Se}, \mathrm{Te}
```

---


## Using the data

The `.gnu` files follow the Quantum ESPRESSO `plotband.x` format: two whitespace-
separated columns (*k*-path coordinate and energy in eV), with each band written
as a block separated by a blank line.

```python
import numpy as np
import matplotlib.pyplot as plt

# read a .gnu file into a list of bands
bands, current = [], []
for line in open("Binary_and_ternary_data/WMoS2_data/Mo4S8_data/bands.out.gnu"):
    s = line.strip()
    if not s:
        if current:
            bands.append(np.array(current)); current = []
        continue
    k, e = map(float, s.split()[:2])
    current.append((k, e))
if current:
    bands.append(np.array(current))

for b in bands:
    plt.plot(b[:, 0], b[:, 1], color="C0", lw=1)

# Γ, M, K, Γ tick positions for the hexagonal path
plt.xticks([0.0000, 0.5774, 0.9107, 1.5774], [r"$\Gamma$", "M", "K", r"$\Gamma$"])
plt.ylabel("E (eV)")
plt.show()
```

The optical CSVs load directly with `pandas.read_csv`.

---


### Using the CASTEP data

The CASTEP band-structure CSV files contain two numerical columns:

1. Normalized *k*-path coordinate
2. Electronic energy in eV

For the present CASTEP dataset, the Γ–M–K–Γ path is normalized from 0 to 1,
with approximate high-symmetry positions:


Γ = 0.000000
M = 0.366049
K = 0.577366
Γ = 1.000000



## Citation

If you use this dataset in your work, please cite **both** the paper and the
dataset itself.

### 1. The dataset

> T. A. Aditto, V. Chowdhury, H. Imtiaz and A. Zubair,
> *TMD alloys electrical and optical dataset*, GitHub repository, 2026.
> https://github.com/ahmedzubair003/TMD_alloys_electrical-_and_optical_dataset

```bibtex
@misc{Zubair2026TMDdataset,
    author       = {Aditto, Tarvir Anjum and Chowdhury, Vivek and Imtiaz, Hafiz and Zubair, Ahmed},
    title        = {{TMD} alloys electrical and optical dataset},
    year         = {2026},
    publisher    = {GitHub},
    journal      = {GitHub repository},
    howpublished = {\url{https://github.com/ahmedzubair003/TMD_alloys_electrical-_and_optical_dataset}},
    note         = {Accessed: YYYY-MM-DD}
}
```

### 2. The accompanying paper

> T. A. Aditto, V. Chowdhury, H. Imtiaz and A. Zubair,
> *Machine learning-enabled prediction of the electronic band-edge shapes and
> properties of 2D transition metal dichalcogenide alloys*,
> **Materials Advances**, 2026, **7**(7), 3767–3780.
> DOI: [10.1039/d5ma01485a](https://doi.org/10.1039/d5ma01485a)

```bibtex
@article{Aditto2026TMDalloys,
    author  = {Aditto, Tarvir Anjum and Chowdhury, Vivek and Imtiaz, Hafiz and Zubair, Ahmed},
    title   = {Machine learning-enabled prediction of the electronic band-edge shapes and properties of 2D transition metal dichalcogenide alloys},
    journal = {Materials Advances},
    volume  = {7},
    number  = {7},
    pages   = {3767--3780},
    year    = {2026},
    month   = {04},
    issn    = {2633-5409},
    doi     = {10.1039/d5ma01485a},
    url     = {https://doi.org/10.1039/d5ma01485a}
}
```

---

## Related work

The dataset underpins the extra-trees model reported in the paper above, which
predicts full conduction and valence band structures — rather than band gaps
alone — for binary and ternary TMD alloys, including compositions outside the
training set.

---

## Contact

Questions and issues are welcome via the
[issue tracker](https://github.com/ahmedzubair003/TMD_alloys_electrical-_and_optical_dataset/issues).

**Ahmed Zubair** — Department of Electrical and Electronic Engineering,
Bangladesh University of Engineering and Technology (BUET)
