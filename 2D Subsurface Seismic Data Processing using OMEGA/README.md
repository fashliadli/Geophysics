# 2D Seismic Reflection Data Processing with OMEGA

Internship project at the Pertamina Upstream Technology Center (UTC), Jakarta, during my B.Sc. in Geophysical Engineering at Institut Teknologi Bandung.

> **Note on documentation:** The full internship report in the `documentation/` folder is written in Indonesian (Bahasa Indonesia). The methods, processing steps and results are summarised below in English.

---

## Project overview

| | |
|---|---|
| **Role** | Geophysics data processing intern (one-month internship, 23 attendance days recorded) |
| **Organisation** | Pertamina Upstream Technology Center (UTC), Jakarta |
| **Period** | 9 July – 9 August 2018 |
| **Input data** | Raw 2D seismic data in SEG-Y format, with the observer report; navigation (ASCII) file made by the acquisition team |
| **Software** | OMEGA seismic processing software; PickWorks Lite (inside OMEGA) for first-break picking |
| **Result** | Stacked seismic sections: one with preserved amplitudes and one with AGC (non-preserved) |
| **Skills used** | Seismic reflection processing, filtering, denoising, deconvolution, static corrections, velocity analysis, migration, stage-by-stage quality control (QC) |

## Goals

1. Learn the seismic processing workflow used in industry.
2. Turn raw field data into a seismic section that shows the subsurface and is ready for interpretation.

## Why processing is needed

Raw field recordings cannot show the subsurface directly. They contain noise (for example background noise and ground roll), amplitudes that fade with distance because the wave energy spreads out (spherical divergence), and timing shifts caused by elevation and the weathered near-surface layer. Processing removes or reduces these effects step by step, so that reflections from the subsurface layers become visible in a section.

---

## Workflow

The workflow had 18 stages (see the flowchart in the gallery; navigation and geometry are described together here). After every stage, the result was checked (QC) on stacks, shot gathers and CMP gathers. Before the first velocity analysis, the QC stacks used a single velocity taken from the average velocities.

| # | Stage | What it does (as described in the report) |
|---|---|---|
| 1 | **Reformatting** | Converts the SEG-Y data to the `.DIO` format that OMEGA processes. |
| 2 | **Navigation and geometry** | Gives each trace its receiver, shot and station number and the survey configuration, taken from an ASCII file made by the acquisition team. |
| 3 | **Static correction** | First breaks are picked on every shot gather with PickWorks Lite; the picks are used for the static correction, which removes the effects of elevation and the weathered (soft) near-surface layer. |
| 4 | **True amplitude recovery (TAR)** | Restores amplitude and energy lost to spherical divergence. |
| 5 | **Digital filtering** | Suppresses unwanted frequencies; here, signals affected by aliasing. |
| 6 | **Denoise (AAA, anomalous amplitude attenuation)** | Looks at a window of data and removes amplitudes judged to be anomalous. It works well where noise is present but not dominant. |
| 7 | **Predictive deconvolution** | Reduces the effect of the source wavelet, to move closer to the reflection coefficients. Two window sizes were compared (4 and 32): window 32 made deeper data clearer, while window 4 resolved shallow data better. Window 32 was used because the target was deeper. |
| 8 | **Denoise after deconvolution** | The same denoise step, repeated on the deconvolved data. Noise is reduced further. |
| 9 | **1st velocity analysis** | Velocities are picked on the semblance (highest energy) so that the events in the CMP gather become flat. QC windows: mini stack, brute stack and NMO isovelocity display. The resulting stack was **worse** than the single-velocity stack because the picking was not accurate enough. |
| 10 | **1st residual statics** | Corrects short-wavelength statics that elevation and refraction statics did not solve, by shifting shots and receivers in time so that neighbouring traces line up. |
| 11 | **2nd velocity analysis** | Velocity analysis repeated after the 1st residual statics. |
| 12 | **2nd residual statics** | Residual statics repeated after the 2nd velocity analysis. |
| 13 | **SCAC (surface-consistent amplitude compensation)** | On the left side of the section, almost no data was visible below about 1000 ms, possibly because hard rock near the surface reflected most of the energy. After SCAC, data became visible there and comparable with the right side. |
| 14 | **Denoise on offset** | Denoise on common-offset gathers: noise that is concentrated in CMP or shot gathers is spread out there, so more of it can be removed. |
| 15 | **Pre-stack time migration (PSTM)** | Moves reflectors to their correct position and removes diffraction effects. Done before stacking, so the data stay amplitude-preserved and velocity analysis is still possible afterwards. |
| 16 | **3rd velocity analysis** | Velocity analysis on the migrated data. After this stage, the processing is considered finished; amplitudes are preserved, so analyses such as AVO remain possible. |
| 17 | **Post-stack processing** | TVF (time-varying filter) to reduce ambient noise, and AGC (automatic gain control) to balance amplitudes over the whole section. |

---

## Results

Two final sections were produced:

- **Preserved-amplitude section** (migrated stack after the 3rd velocity analysis, shown after TVF). Amplitudes keep their original values, so the section can be used for further analysis such as AVO.
- **Non-preserved-amplitude section** (stack after AGC). Amplitudes are balanced over the whole section, which makes horizons easier to pick for an interpreter, but the amplitudes no longer represent the real values.

The sections show information down to a considerable depth. Their resolution is limited because the data contain mainly low frequencies; the report notes that this low-frequency character is typical of the area.

## Visual gallery

### The processing workflow
![Processing workflow](assets/workflow-flowchart.png)

### Raw data and velocity analysis QC

| Raw shot gather (SEG-Y converted to DIO) | Velocity analysis screen (semblance, CMP gather and QC stack panels) |
|---|---|
| ![Raw data](assets/raw-seismic-gather.png) | ![Velocity QC](assets/velocity-analysis-qc.png) |

### Final sections

| Preserved amplitude (after TVF) | Non-preserved amplitude (after AGC) |
|---|---|
| ![Preserved stack](assets/final-stack-tvf.png) | ![AGC stack](assets/final-stack-agc.png) |

---

## What I learned

The conclusions of the report, in short:

- Seismic processing follows a fairly standard sequence of steps, but every data set needs its own handling depending on the problems it shows.
- The parameters depend on the target. For example, the deconvolution window of 32 was chosen to bring out deeper data, at the cost of shallow resolution compared with window 4.
- Understanding the geology of the survey area is needed, especially during velocity analysis.
- Velocity picking needs practice: the first velocity analysis gave a worse stack than the single velocity because the picks were not accurate. The report recommends more picking practice and more study of the geology of the survey area.

## Repository contents

```text
.
├── README.md
├── documentation/   # full internship report (Indonesian)
└── assets/          # figures used in this README
```

## Author

Fashli Adli Wal Ikhsan · [github.com/fashliadli](https://github.com/fashliadli)
