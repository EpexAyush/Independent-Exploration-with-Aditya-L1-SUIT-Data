# ☀️ Aditya-L1 SUIT Solar Flare Observation & Active Region Segmentation

A Python-based scientific data analysis pipeline for processing **Level-1 Near-Ultraviolet (NUV) observations** from the **Solar Ultraviolet Imaging Telescope (SUIT)** aboard India's **Aditya-L1** solar mission.

This project analyzes the **X6.3 solar flare observed on February 22, 2024**, using the **SUIT NB04 filter centered at 279.6 nm**. The pipeline processes FITS observations, cleans and normalizes solar intensity data, segments high-energy active regions, extracts physical parameters, and generates scientific visualizations and time-lapse animations.

---

## 🔭 Project Overview

Solar flares are powerful bursts of electromagnetic radiation associated with highly dynamic processes in the solar atmosphere. Studying their spatial structure and intensity helps improve our understanding of solar activity and **space weather**.

This project implements a complete **Segmentation & Active-region Detection (SSAD)** workflow to identify bright flare regions from the quiet solar disk and quantitatively analyze their physical characteristics.

The analysis uses observations of **Active Region AR 3590** during the X6.3 flare event.

### Dataset

| Parameter        | Details                       |
| ---------------- | ----------------------------- |
| Mission          | **Aditya-L1**                 |
| Instrument       | **SUIT**                      |
| Filter           | **NB04**                      |
| Wavelength       | **279.6 nm**                  |
| Event            | **X6.3 Solar Flare**          |
| Observation Date | **22 February 2024**          |
| Target           | **Active Region AR 3590**     |
| Data Format      | **FITS Level-1 observations** |

---

## 🎯 Objectives

The pipeline was developed to:

* Process Level-1 FITS observations from SUIT.
* Normalize and validate observation metadata.
* Clean invalid `NaN` and `Inf` intensity values.
* Perform Min-Max intensity normalization.
* Analyze pixel intensity distributions using logarithmic histograms.
* Detect bright active regions using **Otsu adaptive thresholding**.
* Remove noise using **morphological opening**.
* Convert pixel measurements into physical solar dimensions.
* Calculate active-region surface area in `km²`.
* Express the active-region footprint in **Millionths of Solar Hemisphere (MSH)**.
* Calculate the flare brightness enhancement ratio.
* Generate annotated solar-coordinate overlays.
* Create a **30 FPS time-lapse animation** of active-region evolution.

---

## ⚙️ Processing Pipeline

```text
Aditya-L1 SUIT Level-1 FITS Data
              │
              ▼
      Metadata Validation
              │
              ▼
     Wavelength Normalization
          (279.6 nm)
              │
              ▼
       Data Cleaning
      NaN / Inf Handling
              │
              ▼
    Min-Max Normalization
          [0.0 – 1.0]
              │
              ▼
   Intensity Distribution
      & Histogram Analysis
              │
              ▼
      Otsu Thresholding
              │
              ▼
   Morphological Opening
        (Disk Radius 3)
              │
              ▼
   Active Region Segmentation
              │
              ▼
      Physical Metrics
   ┌──────────┼──────────┐
   ▼          ▼          ▼
 Area (km²)   MSH    Brightness Ratio
              │
              ▼
     Scientific Visualizations
              │
              ▼
       Time-Lapse Animation
```

---

## 🧪 Methodology

### 1. FITS Data Ingestion & Metadata Normalization

The pipeline loads Level-1 FITS observations using the **SunPy** framework.

The observation metadata is validated and the wavelength information is standardized to:

```text
279.6 nm
```

This ensures consistent processing of the SUIT NB04 observations.

---

### 2. Data Cleaning & Normalization

Raw CCD observations may contain invalid numerical values and strong intensity variations.

Invalid values are handled before normalization:

```python
clean_data = np.nan_to_num(
    data,
    nan=0.0,
    posinf=0.0,
    neginf=0.0
)
```

The intensity distribution is then normalized using Min-Max scaling:

```text
I_norm = (I - I_min) / (I_max - I_min)
```

resulting in an intensity range of:

```text
[0.0, 1.0]
```

---

### 3. Active Region Segmentation

The normalized solar image is segmented using **Otsu's thresholding algorithm**.

Otsu thresholding separates high-intensity flare emission from the comparatively darker solar background.

A morphological opening operation is subsequently applied using a disk-shaped structuring element:

```python
from skimage.filters import threshold_otsu
from skimage.morphology import opening, disk

otsu_threshold = threshold_otsu(normalized_data)

active_region_mask = opening(
    normalized_data > otsu_threshold,
    disk(3)
)
```

This helps remove isolated noise pixels and produces cleaner active-region boundaries.

---

## 📊 Quantitative Results

The analysis produced the following measurements for the segmented active region:

| Metric                     |              Result |
| -------------------------- | ------------------: |
| Spatial Resolution         | **420.91 km/pixel** |
| Segmented Pixels           |           **3,842** |
| Physical Surface Area      |  **6.81 × 10⁸ km²** |
| Solar Hemisphere Footprint |      **223.85 MSH** |
| Peak Brightness Ratio      |           **8.45×** |

The measured brightness enhancement represents the peak flare intensity relative to the quiet solar disk baseline.

---

## 🖼️ Visual Outputs

The project generates multiple scientific visualization artifacts, including:

### Raw Solar Observation

Visualization of the SUIT NB04 solar observation showing the localized high-intensity active region.

### Intensity Distribution

A logarithmic pixel-intensity histogram is generated to study the distribution of solar emission and identify the threshold used for segmentation.

### Binary Segmentation Mask

The processed binary mask highlights the detected active regions after Otsu thresholding and morphological cleanup.

### WCS Active Region Overlay

The segmented boundary is overlaid on the solar **World Coordinate System (WCS)** map to spatially identify **Active Region AR 3590**.

### Time-Lapse Animation

Multiple observations are chronologically processed and compiled into a **30 FPS GIF animation** to visualize the temporal evolution of the active region.

---

## 📁 Repository Structure

```text
.
├── data/
│   ├── raw/
│   │   └── *.fits
│   │
│   └── processed/
│       └── *.npy
│
├── notebooks/
│   └── main_analysis.ipynb
│
├── outputs/
│   ├── figures/
│   │   ├── raw_observation.png
│   │   ├── intensity_histogram.png
│   │   ├── segmentation_mask.png
│   │   └── active_region_overlay.png
│   │
│   ├── animations/
│   │   └── active_region_evolution.gif
│   │
│   └── quantitative_metrics.json
│
├── README.md
└── requirements.txt
```

---

## 🛠️ Technology Stack

### Programming

* **Python**

### Scientific Computing

* NumPy
* SciPy
* SunPy

### Image Processing

* scikit-image
* Matplotlib

### Astronomical Data

* FITS
* Solar Coordinate / WCS processing
* Aditya-L1 SUIT observations

### Analysis Techniques

* Min-Max normalization
* Log-scale intensity analysis
* Otsu thresholding
* Morphological image processing
* Spatial area estimation
* Solar coordinate transformation

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/<repository-name>.git
cd <repository-name>
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

On Linux/macOS:

```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Analysis

Open the main Jupyter Notebook:

```bash
jupyter notebook notebooks/main_analysis.ipynb
```

Run the notebook sequentially to reproduce the preprocessing, segmentation, quantitative analysis, and visualization workflow.

---

## 📈 Key Findings

The pipeline successfully processed SUIT NB04 observations and isolated the high-intensity active region associated with the **February 22, 2024 X6.3 solar flare**.

The analysis produced:

* A segmented active-region mask.
* Physical surface-area estimation.
* MSH footprint estimation.
* Peak brightness enhancement measurement.
* WCS-based spatial visualization.
* Temporal visualization through a 30 FPS animation.

These outputs demonstrate how computational image-processing techniques can be applied to solar observational data for **heliophysics and space-weather analysis**.

---

## ⚠️ Limitations

The current analysis has some physical and methodological limitations:

### Solar Limb Projection

Geometric projection effects can introduce area estimation errors, particularly for regions located away from the center of the solar disk.

### Single-Passband Analysis

The analysis primarily uses the **SUIT NB04 279.6 nm** channel. A multi-wavelength analysis could provide additional information about the thermal and magnetic properties of the flare.

### Future Improvements

Potential future extensions include:

* Multi-channel SUIT analysis.
* EUV observations.
* Magnetogram integration.
* Magnetic flux analysis.
* Spherical projection corrections.
* Automated detection across multiple solar-flare events.
* Comparative analysis of different X-class flares.

---

## 📚 References

1. Ramaprakash et al., *The Solar Ultraviolet Imaging Telescope (SUIT) onboard Aditya-L1 Mission*, Current Science, 2017.
2. SunPy Community, *SunPy — Python for Solar Physics*, The Astrophysical Journal, 2020.
3. N. Otsu, *A Threshold Selection Method from Gray-Level Histograms*, IEEE Transactions on Systems, Man, and Cybernetics, 1979.
4. ISRO Indian Space Science Data Centre (ISSDC), *PRADAN Data Access Manual for Aditya-L1 Payloads*, 2024.

---

## 👨‍💻 Author

**Ayush Kumar**
Indian Institute of Technology, Patna

This project was developed as an **independent scientific exploration** focused on astronomy, astrophysics, heliophysics, and computational analysis of Aditya-L1 observations.

---

## ⭐ Project Highlights

```text
Mission       → Aditya-L1
Instrument    → SUIT
Wavelength    → 279.6 nm
Event         → X6.3 Solar Flare
Region        → AR 3590
Method        → Otsu + Morphological Segmentation
Analysis      → Solar Area + MSH + Brightness
Visualization → WCS Overlay + 30 FPS Animation
Language      → Python
```

---

> **Note:** This repository is intended for scientific exploration, educational use, and reproducible analysis of solar observational data.
