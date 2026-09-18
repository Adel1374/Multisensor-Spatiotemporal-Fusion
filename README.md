# Advanced Multi-Sensor Spatiotemporal Fusion & 6-Band Patch Generation Pipeline

This repository provides an advanced, automated Python pipeline developed in Google Colab for automated querying, synchronized multi-sensor pairing, rigorous cloud/defect filtering, and dataset generation using **Sentinel-2** and **Sentinel-3** satellite imagery. 

Unlike standard benchmark configurations, this pipeline is specifically engineered to construct custom **6-band multi-sensor training patches** optimized for deep learning-based spatial super-resolution and spatiotemporal fusion models.

---

## 🔬 Core Technical Highlights & Innovations

### 1. Dual-Sensor Sentinel-3 Integration (OLCI & SLSTR)
While traditional datasets often rely on single-instrument configurations, this pipeline uniquely merges data streams from **two distinct Sentinel-3 instruments**:
* **OLCI (Ocean Land Color Instrument):** Provides high-sensitivity optical bands.
* **SLSTR (Sea and Land Surface Temperature Radiometer):** Captures complementary spectral and thermal bands.
By combining selected bands from OLCI and SLSTR alongside Sentinel-2, the pipeline builds a synchronized, high-fidelity **6-band feature space** designed to feed advanced neural network architectures.

### 2. Strict Temporal Proximity & Defective Pixel Elimination
To ensure high training data quality, the pipeline implements stringent pre-processing filters:
* **Tight Temporal Windowing:** Sentinel-2 and Sentinel-3 acquisitions are paired within a strict temporal threshold (e.g., narrow-hour windows) to minimize atmospheric and illumination discrepancies.
* **Advanced Masking (SCL & NaN Filtering):** Utilizing Sentinel-2's Scene Classification Layer (SCL), clouds, shadows, cloud-shadows, and other bad/defective pixels are systematically flagged and converted to `NaN`. Both paired images undergo rigorous screening to ensure zero invalid data corrupts the training patches.

### 3. Multi-Scale Patch Generation
The extracted GeoTIFF outputs are structured to support flexible **patch generation across various spatial dimensions and resolutions**:
* **Sentinel-2 High-Resolution Grid:** Processed at **50m** resolution.
* **Sentinel-3 Coarse-Resolution Grid:** Processed at **500m** resolution (maintaining a precise 1:10 scaling ratio relative to Sentinel-2).
* Tailored for generating uniform spatial patches used in super-resolution (SR) and multi-sensor downscaling tasks.

---

## 🛰️ Reference & Background
This work builds upon and extends spatial-temporal concepts introduced in recent remote sensing literature:
> *Earth Science Informatics (2025) 18:349* - [A new benchmark for spatiotemporal fusion of Sentinel-2 and Sentinel-3 OLCI images](https://doi.org/10.1007/s12145-025-01855-4).

---

## ⚙️ Pipeline Workflow

1. **Workspace Setup & Drive Mounting:** Initializes secure Google Colab storage.
2. **Library Installation:** Sets up geospatial packages (`sentinelhub`, `rasterio`, `pyproj`, `pandas`, etc.).
3. **Copernicus Authentication:** Connects securely to the Copernicus Data Space Ecosystem API.
4. **Benchmark Region & BBox Extraction:** Computes UTM bounding boxes and central coordinates.
5. **Multi-Year Bulk Search & Matching:** Executes large-scale queries (2018–2025) with integrated **Resume Logic**.
6. **Smart Download & 6-Band Masking:** Downloads, masks defective pixels, and packages dual-sensor data into standardized 6-band GeoTIFF patches.
7. **Visual Quality Inspection:** Automatically validates sample integrity and plots side-by-side RGB comparisons.

---

## 📂 Repository Structure

```text
├── search_results_FINAL.csv      # Log metadata of temporally-matched S2/S3 pairs
├── notebook_pipeline.ipynb       # End-to-end execution notebook
└── README.md                     # Project documentation