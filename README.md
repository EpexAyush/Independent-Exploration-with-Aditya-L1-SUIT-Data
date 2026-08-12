# Aditya-L1 SUIT Solar Flare Observation & Active Region Segmentation Pipeline

A comprehensive Python-based data processing and spatial analysis pipeline designed for Level-1 Near-Ultraviolet (`NB04` filter centered at $279.6\text{ nm}$) solar observations captured by the **Solar Ultraviolet Imaging Telescope (SUIT)** aboard India's flagship solar mission, **Aditya-L1**.

This repository focuses on processing observations during the extreme **X6.3 Solar Flare event** on **February 22, 2024**. The pipeline isolates high-energy active region flare cores from the quiet solar disk, performs pixel-to-kilometer coordinate transformations, quantifies physical solar metrics, and renders dynamic visualizations and temporal animations.

---

## 📌 Project Overview & Objectives

Solar flares release massive bursts of electromagnetic radiation, primarily driven by complex magnetic field reconfigurations in Active Regions (ARs). Processing high-resolution UV imagery from Aditya-L1 allows space weather scientists to track flare boundary dynamics, compute surface areas, and measure intensity enhancements.

**Core Objectives:**
1. **Metadata Normalization:** Programmatically correct and standardize missing FITS header wavelengths ($279.6\text{ nm}$).
2. **Preprocessing & Artifact Cleaning:** Handle non-physical sensor artifacts (`NaN`/`Inf`) and apply Min-Max intensity normalization $[0.0, 1.0]$.
3. **Adaptive Active Region Segmentation:** Utilize Otsu thresholding on log-scale intensity distributions combined with morphological opening to isolate continuous flare cores.
4. **Quantitative Metric Extraction:** Calculate the physical surface area ($km^2$), Millionths of Solar Hemisphere (MSH) footprint, and peak brightness enhancement ratio.
5. **Visual Artifact Generation:** Produce high-resolution annotated overlay figures and a 30 FPS flicker-free time-lapse GIF animation.

---

## 📁 Project Directory Structure

```text
.
├── data/
│   ├── raw/                 # Input Aditya-L1 Level-1 FITS files (*.fits)
│   └── processed/           # Exported preprocessed NumPy matrices (.npy)
├── notebooks/
│   └── main_analysis.ipynb  # Comprehensive 5-Step Jupyter Notebook pipeline
├── outputs/
│   ├── figures/             # Output plots, histograms, and contour overlays
│   ├── animations/          # 30 FPS active region evolution GIF
│   └── quantitative_metrics.json  # Calculated physical metrics (JSON)
└── README.md                # Project documentation