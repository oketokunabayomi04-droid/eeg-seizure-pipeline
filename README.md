# EEG Seizure Detection — A Reproducible Signal Processing Pipeline

This project explores how scalp EEG signals change during epileptic 
seizures. Starting from raw clinical recordings, the pipeline moves 
through signal loading, preprocessing, feature extraction, annotation 
alignment, and baseline classification.

## Dataset

CHB-MIT Scalp EEG Database (PhysioNet)  
24 paediatric participants with epilepsy  
Shoeb, A. H. (2009). MIT PhD Thesis.  
https://physionet.org/content/chbmit/1.0.0/

## Pipeline overview

1. Parse all participant summary files into a recording manifest
2. Verify seizure annotations before loading any signal
3. Load and bandpass filter the EEG (1–40 Hz)
4. Extract seven features from 10-second non-overlapping windows
5. Label each window against documented seizure intervals
6. Train a Random Forest classifier with stratified cross-validation
7. Evaluate with precision, recall, F1, and confusion matrix

## Key result (single recording — chb01_15.edf)

- Recording duration: 60 minutes at 256 Hz across 23 channels
- Seizure interval: 1732–1772 seconds
- Windows: 360 total (355 non-seizure, 5 seizure)
- Cross-validated F1: 1.00 on both classes within this recording

The perfect within-recording score reflects the severe class imbalance 
(71:1) and the small number of seizure windows per fold. It is an 
expected result at this stage, not a generalisation claim. The next 
step is a participant-level train/test split across the full dataset.

## Feature importance

| Feature | Importance |
|---|---|
| RMS Amplitude | 0.2960 |
| Variance | 0.2671 |
| Delta Power (1–4 Hz) | 0.1732 |
| Theta Power (4–8 Hz) | 0.0999 |
| Line Length | 0.0902 |
| Beta Power (13–30 Hz) | 0.0732 |
| Alpha Power (8–13 Hz) | 0.0003 |

RMS and variance dominating is consistent with the known physiology: 
seizures involve large-amplitude, synchronised bursts of neural activity. 
Alpha power contributing almost nothing aligns with the fact that alpha 
rhythms are typically suppressed during ictal activity.

## Tools

Python · MNE · scikit-learn · SciPy · NumPy · pandas · Matplotlib

## Full notebook

(https://www.kaggle.com/code/oketokunabayomi/understanding-eeg-signal-changes-during-epileptic/output)

## Author

Oketokun Abayomi Tolulope
