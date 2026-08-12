# 🌞Aditya-L1 SUIT Solar Flare Analysis & Active Region Segmentation

A Python-based scientific data analysis project for processing **Level-1 FITS observations** from the **Solar Ultraviolet Imaging Telescope (SUIT)** aboard India's **Aditya-L1** solar mission.

This project investigates the **X6.3-class solar flare observed on February 22, 2024**, using SUIT's **Near-Ultraviolet (NUV) NB04 filter at 279.6 nm**. The analysis focuses on detecting and segmenting high-intensity solar active regions, extracting physical parameters, and visualizing their spatial and temporal characteristics.

---

## 🔭 Overview

Solar flares are high-energy events occurring in active regions of the solar atmosphere. Their observation and analysis are important for understanding solar activity and **space weather**.

This project implements an **SSAD (Segmentation & Active-region Detection)** pipeline that processes SUIT observations and separates high-intensity flare regions from the surrounding solar disk.

The selected dataset covers observations from **22:00:00 to 23:59:00 UTC on February 22, 2024**, during the X6.3 flare event. The NB04 filter, centered at **279.6 nm**, provides NUV observations suitable for analyzing bright flare emission and active-region boundaries.

---

## 🎯 Project Objectives

The pipeline is designed to:

* Ingest and process Level-1 FITS observations.
* Validate and normalize observation metadata.
* Standardize the observing wavelength to **279.6 nm**.
* Clean invalid `NaN` and `Inf` values.
* Apply **Min-Max intensity normalization**.
* Analyze pixel intensity distributions using log-scale histograms.
* Detect active regions using **Otsu thresholding**.
* Remove noise using morphological opening.
* Calculate physical active-region surface area.
* Express the area in **Millionths of Solar Hemisphere (MSH)**.
* Calculate peak flare brightness enhancement.
* Generate WCS-based scientific visualizations.
* Produce a time-series GIF showing active-region evolution.

---

## 🧪 Analysis Pipeline

```text
Level-1 FITS Observations
          │
          ▼
Metadata Validation
          │
          ▼
Wavelength Normalization
        279.6 nm
          │
          ▼
Data Cleaning
     NaN / Inf
          │
          ▼
Min-Max Normalization
       [0.0, 1.0]
          │
          ▼
Log-Scale Histogram Analysis
          │
          ▼
Otsu Thresholding
          │
          ▼
Morphological Opening
       Disk (3 px)
          │
          ▼
Active Region Mask
          │
          ▼
Physical Parameter Extraction
     ┌────┼────┐
     ▼    ▼    ▼
   Area  MSH  Brightness
          │
          ▼
Scientific Visualizations
          │
          ▼
Temporal GIF Animation
```

---

## ⚙️ Methodology

### 1. FITS Data & Metadata Normalization

Raw Level-1 FITS observations are loaded using the **SunPy** framework.

During metadata validation, the observing wavelength was found to be missing or uncalibrated in the raw header. The pipeline therefore applies a programmatic metadata correction and explicitly sets the observing wavelength to **279.6 nm**.

### 2. Preprocessing

Raw observations can contain invalid numerical values, cosmic-ray spikes, and strong intensity variations.

The pipeline:

1. Replaces `NaN` and `Inf` values.
2. Normalizes image intensity using Min-Max scaling.
3. Generates a log-scale intensity histogram for distribution analysis.

The normalized intensity is calculated as:

```text
I_norm = (I - I_min) / (I_max - I_min)
```

resulting in a normalized range of `[0.0, 1.0]`.

### 3. Active Region Segmentation

The normalized image is segmented using **Otsu's thresholding algorithm**, which separates high-intensity flare emission from the background solar disk.

A morphological opening operation with a **disk-shaped structuring element of radius 3 pixels** is then applied to reduce isolated noise and improve the detected boundaries.

Core implementation:

```python
import numpy as np
from skimage.filters import threshold_otsu
from skimage.morphology import opening, disk

clean_data = np.nan_to_num(
    smap.data,
    nan=0.0,
    posinf=0.0,
    neginf=0.0
)

norm_data = (
    clean_data - np.min(clean_data)
) / (
    np.max(clean_data) - np.min(clean_data)
)

otsu_thresh = threshold_otsu(norm_data)

active_region_mask = opening(
    norm_data > otsu_thresh,
    disk(3)
)
```

---

## 📐 Quantitative Analysis

The project investigates the physical properties of **Active Region AR 3590** during the selected X6.3 flare event.

Using the FITS metadata and solar reference parameters, the calculated spatial resolution is approximately **420.91 km/pixel**. The segmented mask contains **3,842 pixels**.

### Results

| Parameter                  |               Value |
| -------------------------- | ------------------: |
| Spatial Resolution         | **420.91 km/pixel** |
| Active Region Pixels       |           **3,842** |
| Physical Surface Area      |  **6.81 × 10⁸ km²** |
| Solar Hemisphere Footprint |      **223.85 MSH** |
| Peak Intensity Ratio       |           **8.45×** |

The peak intensity ratio represents the flare-core intensity relative to the quiet solar disk median baseline.

---

## 🖼️ Visualizations

The project generates several visualization outputs:

### Raw Observation

Displays the Level-1 SUIT NB04 observation and the localized high-intensity solar region.

### Intensity Distribution

A log-scale histogram is used to inspect the pixel-intensity distribution and evaluate the Otsu threshold boundary.

### Segmentation Mask

The cleaned binary mask shows the active regions detected after thresholding and morphological processing.

### WCS Overlay

The segmented active-region boundary is overlaid on the solar **World Coordinate System (WCS)** map, with annotations targeting **AR 3590**.

### Time-Lapse Animation

Multiple FITS frames recorded between **22:00 and 23:59 UTC** are chronologically processed using fixed intensity scaling and compiled into a **30 FPS GIF** to visualize temporal changes in the active region.

---

## 📁 Repository Structure

```text
.
├── data/
│   ├── raw/
│   │   └── *.fits
│   └── processed/
│       └── *.npy
│
├── notebooks/
│   └── main_analysis.ipynb
│
├── outputs/
│   ├── figures/
│   ├── animations/
│   └── quantitative_metrics.json
│
├── README.md
└── requirements.txt
```

---

## 🛠️ Technologies & Libraries

* **Python**
* **NumPy**
* **SunPy**
* **scikit-image**
* **Matplotlib**
* **FITS**
* **WCS / Solar Coordinate Analysis**

### Key Techniques

* FITS data processing
* Metadata normalization
* Min-Max normalization
* Log-scale histogram analysis
* Otsu thresholding
* Morphological image processing
* Active-region segmentation
* Solar surface-area estimation
* WCS visualization
* Time-series image processing

---

## 🚀 Getting Started

### Clone the Repository

```bash
git clone https://github.com/<username>/<repository-name>.git
cd <repository-name>
```

### Create a Virtual Environment

**Windows**

```bash
python -m venv venv
venv\Scripts\activate
```

**Linux / macOS**

```bash
python3 -m venv venv
source venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run the Analysis

Launch the Jupyter Notebook:

```bash
jupyter notebook notebooks/main_analysis.ipynb
```

Run the notebook cells sequentially to reproduce the processing and analysis workflow.

---

## 📌 Key Findings

For the selected **AR 3590 X6.3 flare observation**, the analysis produced an estimated active-region footprint of **223.85 MSH**, corresponding to approximately **6.81 × 10⁸ km²**, with a peak brightness enhancement of approximately **8.45×** relative to the quiet solar disk baseline.

The resulting segmentation masks, WCS overlays, and time-lapse visualizations provide a computational view of the spatial and temporal characteristics of the observed flare region.

---

## ⚠️ Limitations & Future Work

### Current Limitations

**Limb Foreshortening:**
Projection effects can introduce errors in area calculations for active regions located away from the disk center.

**Single-Passband Analysis:**
The current analysis relies on the **NUV NB04 channel**, which does not provide multi-thermal diagnostics of the flare.

### Future Improvements

Possible extensions include:

* Spherical projection corrections.
* Multi-channel SUIT analysis.
* Integration of EUV observations.
* Magnetogram-based magnetic flux analysis.
* Cross-comparison with additional solar flare events.
* Automated analysis of larger solar-observation datasets.

---

## 📚 References

1. A. N. Ramaprakash et al., *The Solar Ultraviolet Imaging Telescope (SUIT) onboard Aditya-L1 Mission*, Current Science, 2017.
2. The SunPy Community et al., *SunPy—Python for Solar Physics*, The Astrophysical Journal, 2020.
3. N. Otsu, *A Threshold Selection Method from Gray-Level Histograms*, IEEE Transactions on Systems, Man, and Cybernetics, 1979.
4. ISRO Indian Space Science Data Centre (ISSDC), *PRADAN Data Access Manual for Aditya-L1 Payloads*, 2024.

---

## 👨‍💻 Project

**Independent Exploration with Aditya-L1 Data**

**Subject:** Astronomy & Astrophysics — Heliophysics with Aditya-L1
**Author:** Ayush Kumar
**Institute:** Indian Institute of Technology, Patna (IIT Patna)

This work was developed as part of a **Summer Training Program in Space Science & Technology** under the **India Space Academy, Department of Space Education, India Space Week**.

---

⭐ If you find this project useful for learning about **solar physics, astronomical data analysis, or scientific image processing**, consider starring the repository.
