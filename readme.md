# Battery Equivalent Circuit Model (ECM) Simulation

> Python implementation of battery Equivalent Circuit Models (Thevenin / Extended Thevenin) for SOC, SOH, and RUL estimation. Includes cell-level and pack-level simulation with passive/active cell balancing. Based on academic research at TU Berlin and coursework from University of Colorado Boulder.

---

## 🔋 Project Overview

This project provides a complete battery simulation framework that:
- Models individual cells using **1-RC and 2-RC Thevenin ECMs**
- Estimates **State of Charge (SOC)** via Coulomb counting + OCV correction
- Estimates **State of Health (SOH)** from capacity fade and resistance growth
- Predicts **Remaining Useful Life (RUL)** using degradation models
- Simulates **full battery packs** (14S3P) with cell-level parameter variation
- Implements **passive and active cell balancing** strategies
- Includes **thermal modeling** (temperature-dependent resistance)

---

## 🏗️ Architecture

```
battery-ecm-simulation/
├── src/
│   ├── ecm_model.py        # Core ECM: cell model, SOC/SOH/RUL, current profiles
│   ├── pack_simulation.py  # Multi-cell pack + cell balancing
│   └── visualize.py        # Plotting + simulation reports
├── notebooks/
│   └── ecm_analysis.ipynb
├── data/
├── results/
├── requirements.txt
└── README.md
```

---

## ⚙️ Setup

```bash
git clone https://github.com/PRATdoppelEK/battery-ecm-simulation.git
cd battery-ecm-simulation
pip install -r requirements.txt
```

---

## 🚀 Quickstart

### Single cell simulation
```python
from src.ecm_model import BatteryECM, CellParameters, cc_discharge_profile

params  = CellParameters(capacity_ah=50.0, r0=0.002, r1=0.001, c1=3000)
cell    = BatteryECM(params=params, extended=True, dt=1.0)
cell.reset(initial_soc=1.0)

profile = cc_discharge_profile(50, c_rate=0.5)
history = cell.simulate(profile)
```

### Pack simulation with balancing
```python
from src.pack_simulation import BatteryPackSimulator, PackConfiguration

config = PackConfiguration(n_series=14, n_parallel=3, capacity_ah=50)
pack   = BatteryPackSimulator(config)
pack.reset(initial_soc=0.95, soc_spread=0.05)
pack.simulate_pack(profile, balance=True)
```

### Run visualization
```bash
python src/visualize.py
```

---
## 📈 Results

### 50Ah NMC Cell — 0.5C Discharge Simulation

![Cell discharge simulation](assets/screenshots/cell_discharge.png)

### 14S3P Pack — 0.5C Discharge with Cell Balancing

![Pack discharge simulation](assets/screenshots/pack_discharge.png)
---

## 📊 Model Features

| Feature | Description |
|---------|-------------|
| ECM Type | 1-RC and 2-RC Thevenin |
| SOC method | Coulomb counting + OCV lookup |
| Thermal model | Lumped thermal mass, ambient coupling |
| Balancing | Passive (resistive bleed) + Active (charge transfer targets) |
| Current profiles | CC-Discharge, CCCV-Charge, WLTP-like drive cycle |
| SOH model | Capacity fade + resistance growth |
| RUL prediction | Linear degradation extrapolation |

---

## 🧠 Key Technical Highlights

- **OCV–SOC lookup table** for NMC chemistry (interpolated)
- **Discrete-time RC dynamics**: exact exponential integration (no Euler approximation errors)
- **Temperature-dependent R0**: linear correction around 25°C reference
- **Cell SOH variation**: random spread across pack cells for realistic imbalance simulation
- **Validated against** MATLAB/Simulink reference models (TU Berlin thesis, Dan-Tech Energy)

---

## 🎓 Academic Background

- **M.Sc. Thesis**: "Custom Battery Cell Balancing Circuit Design Under Thermal Gradient" — TU Berlin
- **Certifications**: Battery Management Systems (with honors), Equivalent Circuit Cell Model Simulation, Battery Pack Balancing and Power Estimation — University of Colorado Boulder

---

## 🔧 Tech Stack

`NumPy` · `Matplotlib` · `SciPy` · `Pandas` · `Jupyter` · `Python 3.10+`

---

## 👤 Author

**Prateek Gaur** — ML Engineer | Battery & Engineering AI  
[LinkedIn](https://www.linkedin.com/in/prateek-gaur-15a629b4) · [GitHub](https://github.com/PRATdoppelEK)
