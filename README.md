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
