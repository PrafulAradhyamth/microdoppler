# Radar-Based Drone Classification Study Guide

## I. Overview of Drone Classification using Radar Micro-Doppler Data

### A. Introduction to Radar-Based Drone Classification
- Goal: Classify various drone models by analyzing **unique micro-Doppler signatures**.
- Millimeter-wave radars offer **high Doppler sensitivity**, enabling the capture of subtle signatures from small rotating blades.
- Prior work showed that **Continuous Wave (CW) radar** can effectively discriminate drone types using micro-Doppler spectrograms.

### B. Comparison of CW and FMCW Radar for Micro-Doppler

| Radar Type | Advantages | Limitations |
|------------|------------|-------------|
| **CW Radar** | Fully sampled micro-Doppler, clear resolution of blade flashes | Cannot localize the target (no range info) |
| **FMCW Radar** | Can localize target (range measurement) | Often undersamples micro-Doppler → aliasing in spectrograms |

**Project Focus:** Determine if accurate classification is possible using **undersampled FMCW micro-Doppler signatures** despite aliasing.

---

## II. Experimental Setup and Data Collection

### A. Radar System Configuration
- **Type:** 94 GHz radar (configurable CW/FMCW)
- **Bandwidth:** 150 MHz
- **Beam:** Dual fan beams (narrow in azimuth)
- **Sensitivity:** High sensitivity, low noise figure, low phase noise
- **Sampling:** 10 MHz
- **Processing:** 512 fast-time samples for 256 range bins
- **Unambiguous Doppler Velocity:** ±10 m/s
- **Spectrogram CPIs:**
  - Short Window: 512 samples (~40 ms)
  - Long Window: 4 kilopoints (>300 ms)

### B. Drone Types
1. **DJI S900** – Large hexacopter (~1 m)
2. **DJI Inspire 1** – Medium quadcopter (~0.5 m)
3. **DJI Phantom 3** – Small quadcopter (~1 ft)
4. **"Giant" Quadcopter** – Very large quadcopter (~1 m) with larger blades than S900

### C. Data Acquisition & Processing
- Collected at various distances (tens–hundreds of meters)
- **Extraction:** Manually isolated range bins with target during episodes of interest
- **Spectrograms:** Generated for each episode
- Dataset is **small** (proof-of-concept)

---

## III. Feature Extraction from Micro-Doppler Data

### A. Micro-Doppler Strength
- **Calculation:** Short-window CPI spectrograms
- **Bulk Doppler suppression** below noise floor
- Mean micro-Doppler energy from sides of spectrogram, scaled by range offset
- **Purpose:** Quantifies overall micro-Doppler energy

### B. Bulk to Micro-Doppler Ratio
- **Calculation:** Short-window CPI spectrograms
- Compares average micro-Doppler side energy vs. bulk Doppler energy
- **Purpose:** Measures relative strength of bulk vs. micro-Doppler (weakest feature in classification)

### C. Hermline Spacing
- **Calculation:** Long-window CPI STFT spectrograms
- Identifies **roto-modulation lines (Hermlines)** related to blade rotation rate & length
- Finds peaks in ±7 m/s range, computes mean spacing
- **Purpose:** Captures blade size/rotation characteristics

---

## IV. Machine Learning Classification

### A. Methodology
- **Tool:** MATLAB Classification Learner App
- **Classifiers Tested:** 24 total
- **Features:** 47 values × 3 features = **141 per sample**
- **Classes:** S900, Inspire 1, Phantom 3, Giant
- **Split:** 80% training / 20% validation

### B. Results
- **Four-Class:**  
  - Best = **85%** (Quadratic SVM)  
  - Hermline spacing + micro-Doppler strength showed good class separation  
  - Most confusion: **S900 vs Phantom 3** (similar blade lengths)
- **Three-Class:**  
  - Worst (S900 & Phantom both present) ≈ 77%  
  - Best (omit S900 or Phantom) ≈ 97%
- **Two-Class:** Highest accuracies (details not provided)
- **Feature Importance:** Bulk to micro-Doppler ratio = least significant
- **Practical Note:** Prediction speed matters for real-time use

---

## V. Limitations and Future Work

### A. Current Limitations
- Preliminary, **small dataset**
- Frequent S900 ↔ Phantom 3 confusion
- Limited statistical reliability

### B. Future Work
- Collect larger datasets
- Investigate confusion causes (blade length vs. shape)
- Develop more discriminative features

---

## Quiz: Drone Classification with Radar

**Instructions:** Answer each in 2–3 sentences.

1. **Advantage of millimeter-wave radars for micro-Doppler detection?**  
   High Doppler sensitivity captures subtle motion from small blades within short integration times.

2. **Limitation of CW radar?**  
   Cannot localize targets (no range), despite producing excellent micro-Doppler signatures.

3. **Why does FMCW undersample micro-Doppler?**  
   Low chirp repetition rate → aliasing spreads energy across spectrum → blade flashes unresolvable.

4. **Central project question?**  
   Can drone models be discriminated using **undersampled FMCW micro-Doppler** despite aliasing?

5. **Micro-Doppler strength calculation?**  
   Suppress bulk Doppler below noise, average side energy, scale with range offset.

6. **Hermline spacing feature purpose & window?**  
   Captures blade rotation rate/length; calculated using **long-window CPI** STFT.

7. **Best four-class classifier?**  
   Quadratic SVM, **85%** accuracy.

8. **Most confused drones & reason?**  
   S900 & Phantom 3; similar blade lengths despite size differences.

9. **Least significant feature?**  
   Bulk to micro-Doppler ratio.

10. **Other real-time consideration besides accuracy?**  
    Prediction speed for timely responses.

---

## Essay Questions

1. **CW vs FMCW for micro-Doppler:**  
   Compare sampling quality, range localization ability, and aliasing issues. Explain project’s FMCW focus.

2. **Feature descriptions & effectiveness:**  
   Detail extraction method, physical meaning, and observed classification contribution.

3. **Classification results analysis:**  
   Discuss varying accuracies across scenarios, S900 vs Phantom 3 confusion, and class separability insights.

4. **Dataset size significance:**  
   Explain impact on generalization, statistical reliability, and risk of overfitting.

5. **Beyond accuracy – real-time factors:**  
   Discuss prediction speed, computational cost, and deployment feasibility.

---
## VII. FAQ

**Q1: What is the primary objective of this research?**  
The main goal is to determine if different drone models can be accurately classified using **undersampled FMCW radar micro-Doppler signatures**. This addresses CW radar’s limitation of lacking target localization.

**Q2: Why is FMCW radar challenging for micro-Doppler analysis compared to CW radar?**  
FMCW radars often have chirp repetition rates too low to fully sample fast rotor motions, causing **aliasing** and making blade flashes hard to resolve. CW radar, when fully sampled, can clearly capture these flashes.

**Q3: What are the three key features extracted for classification?**  
1. **Micro-Doppler Strength** – Overall micro-Doppler energy after bulk suppression.  
2. **Bulk to Micro-Doppler Ratio** – Relative strength of bulk vs. side micro-Doppler energy.  
3. **Hermline Spacing** – Mean spacing between roto-modulation lines, related to blade size/rotation.

**Q4: What types of drones were used and how was the data collected?**  
Four drones: **S900**, **Inspire 1**, **Phantom 3**, and a **Giant Quadcopter**. Data was collected using a 94 GHz radar at distances from tens to hundreds of meters. Relevant range bins were manually extracted before generating spectrograms.

**Q5: Which machine learning algorithm performed best in four-class classification?**  
The **Quadratic SVM** achieved the highest accuracy of **85%**.

**Q6: Which drones were most often confused, and why?**  
The **Phantom 3** and **S900** were most confused, likely due to similar blade lengths despite different blade shapes.

**Q7: How did accuracy change with fewer drone classes?**  
Three-class accuracy ranged from **77%** (worst, including S900 & Phantom) to **97%** (best, excluding one of them). Two-class combinations generally performed even better.

**Q8: What are the main limitations and next steps?**  
Limitations: small dataset, manual range bin extraction.  
Next steps: collect more data, investigate confusion causes, refine features and classifiers for better accuracy and prediction speed.

---

## Glossary of Key Terms

- **Aliased:** Sampling artifact where high frequencies appear as lower frequencies.
- **Bulk Doppler:** Doppler shift from the drone body motion.
- **Chirp Repetition Frequency (CRF):** FMCW pulse transmission rate.
- **Continuous Wave (CW) Radar:** No modulation; measures Doppler, no range.
- **CPI (Coherent Processing Interval):** Time span for coherent radar data collection.
- **Doppler Sensitivity:** Ability to detect small frequency shifts.
- **FMCW Radar:** Modulated wave; measures both range & velocity.
- **Hermline Spacing:** Distance between roto-modulation peaks in spectrogram.
- **Localization:** Determining a target’s position.
- **Machine Learning (ML):** Systems learning from data for decision-making.
- **MATLAB Classification Learner App:** Tool for ML model training/testing.
- **Micro-Doppler:** Small Doppler fluctuations from moving subcomponents.
- **Micro-Doppler Strength:** Quantified energy in micro-Doppler after bulk suppression.
- **Millimeter-wave Radar:** High-frequency radar for high resolution/sensitivity.
- **Noise Figure:** SNR degradation measure.
- **Phase Noise:** Phase instability degrading Doppler performance.
- **Quadratic SVM:** Non-linear SVM classifier with quadratic kernel.
- **Range Bins:** Discrete distance intervals in radar processing.
- **Spectrogram:** Time-frequency representation of a signal.
- **STFT (Short-Time Fourier Transform):** Time-localized frequency analysis.
- **Undersampling:** Sampling below Nyquist rate causing aliasing.

