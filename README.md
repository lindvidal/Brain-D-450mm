# BRain-D: BRazilian Daily Rainfall Gridded Data Pipeline

[![DOI](https://img.shields.io/badge/DOI-10.1016%2Fj.atmosres.2025.108552-blue)](https://doi.org/10.1016/j.atmosres.2025.108552)

**BRain-D** is a high-resolution (0.1° × 0.1°) daily rainfall gridded dataset and methodology, originally developed for the Brazilian territory (1961–2024). The key innovation of this project is its foundation on a strict, fully **Automatic Quality Control Procedure (A-QCP)** designed to systematically filter out low-quality meteorological records and preserve extreme rainfall events.

While this repository contains the scripts used to generate the BRain-D dataset for Brazil, it is structured as a **reusable pipeline**. Academic researchers, hydrologists, and data scientists can adapt this methodology to apply the A-QCP and Inverse Distance Weighting (IDW) interpolation to their own custom datasets.

---

## 🚀 Using This Pipeline for Your Own Data

The pipeline is split into four sequential phases, represented by the numbered Jupyter notebooks in the `Scripts e Dados/` directory.

### Phase 1: Data Preparation (1.x Notebooks)
If you are bringing your own dataset, your primary goal in this phase is to format your raw data to match the expected HDF5 input structure used by the rest of the pipeline.
- The `1.x` notebooks demonstrate how raw data from various Brazilian agencies (CEMADEN, ANA/Telemetria, INMET) was standardized.
- **Action:** Review the output format of these notebooks (specifically the consolidated HDF5 files) and ensure your custom dataset mirrors this structure (e.g., specific columns for `gauge_code`, `date`, `rain_mm`) before proceeding to Phase 2.

### Phase 2: Automatic Quality Control Procedure (A-QCP) (2.x Notebooks)
This is the core of the methodology. The A-QCP consists of:
1. **Basic Quality Control:** Removes physically implausible values (e.g., negative rainfall or unrealistic extremes based on `parameters.json`) and flags gauges with extensive missing data.
2. **Absolute Quality Control:** Evaluates the annual consistency of rain gauges by calculating a Quality Index (Q) based on completeness, gap distribution, weekday consistency, and outlier frequency.
- **Action:** Run the `2.x` notebooks sequentially on your formatted HDF5 dataset to generate a filtered, high-quality dataset.

### Phase 3: Spatial Interpolation (3.x Notebooks)
Once the data is quality-controlled, the `3.x` notebooks apply an Inverse Distance Weighting (IDW) interpolation method to generate the final regular grid.
- Cross-validation procedures are also included to assess the statistical accuracy of the interpolation (RMSE, Bias, Pearson's R, etc.).

### Phase 4: Long-term Validation (4.x Notebooks)
The `4.x` notebooks provide tools for long-term validation against climatological normals (e.g., 30-year averages), ensuring that the gridded dataset spatially represents long-term precipitation trends accurately.

---

## ⚙️ Configuration (`parameters.json`)

You can customize the physical thresholds for the Basic Quality Control step by editing the `Scripts e Dados/parameters.json` file. The defaults are set for the Brazilian climate:

```json
{
  "thresholds": {
    "upper": 450.0,
    "lower": 0.0,
    "extreme_event": 200.0
  },
  "units": "mm/day"
}
```

---

## 📄 Citation

If you use this methodology or the BRain-D dataset in your research, please cite the original publication in *Atmospheric Research*:

> Vidal-Barbosa, J. L. et al. (2025). **BRain-D: A quality-controlled methodology for constructing the BRazilian Daily rainfall gridded data**. *Atmospheric Research*, 108552. [https://doi.org/10.1016/j.atmosres.2025.108552](https://doi.org/10.1016/j.atmosres.2025.108552)

---

## 💾 Data Access

The raw Brazilian data and the final generated BRain-D gridded datasets are freely available at [www.hydro-hub.com.br](http://www.hydro-hub.com.br) and via the [Zenodo repository](https://doi.org/10.5281/zenodo.15468235).
