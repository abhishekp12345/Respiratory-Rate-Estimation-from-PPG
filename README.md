# Respiratory Rate Estimation from PPG using Deep Learning

## Project Overview

Respiratory Rate (RR) is one of the most important vital signs used to assess a person's respiratory health. Traditional RR measurement methods require dedicated respiratory sensors, which may not always be available in wearable devices.

This project aims to estimate respiratory rate directly from Photoplethysmography (PPG) signals using a deep learning model. A 1D Convolutional Neural Network (CNN) is trained on the BIDMC PPG and Respiration Dataset to predict respiratory rate without using any respiratory sensor during inference.

---

## Dataset

**Dataset:** BIDMC PPG and Respiration Dataset

The dataset contains synchronized recordings of:

- Photoplethysmography (PPG)
- Respiration signal
- ECG

A total of **53 patient recordings** were used.

The respiration signal was used only to generate ground-truth respiratory rate labels, while the CNN model was trained using only PPG signals.

---

## Project Workflow

```
Raw PPG Signal
       │
       ▼
Butterworth Bandpass Filtering
       │
       ▼
Detrending
       │
       ▼
Z-score Normalization
       │
       ▼
Window Segmentation (32 seconds)
       │
       ▼
Respiratory Rate Label Generation
       │
       ▼
Patient-wise Train / Validation / Test Split
       │
       ▼
1D CNN Model
       │
       ▼
Respiratory Rate Prediction
```

---

## Data Preprocessing

The following preprocessing steps were applied:

- Butterworth Bandpass Filter
- Signal Detrending
- Z-score Normalization
- 32-second window segmentation with overlap
- FFT-based respiratory rate label generation
- Removal of invalid windows
- Patient-wise data splitting to avoid data leakage

---

## CNN Architecture

The implemented model consists of:

- Conv1D Layers
- Batch Normalization
- ReLU Activation
- MaxPooling1D
- Dropout
- Global Average Pooling
- Fully Connected Dense Layer
- Linear Output Layer

Loss Function:
- Mean Squared Error (MSE)

Optimizer:
- Adam

Evaluation Metrics:
- MAE
- RMSE
- R² Score
- Pearson Correlation

---

## Results

### Test Set Performance

| Metric | Value |
|---------|-------|
| MAE | **2.45 breaths/min** |
| RMSE | **3.75 breaths/min** |
| Pearson Correlation | **0.44** |
| R² Score | **0.001** |

### Cross Validation Performance

| Metric | Mean ± Std |
|---------|------------|
| MAE | **2.50 ± 0.61** |
| RMSE | **3.16 ± 0.78** |
| Pearson Correlation | **0.24 ± 0.23** |
| R² Score | **-0.15 ± 0.10** |

---

---

## Challenges and Limitations

Although the proposed CNN model successfully estimates respiratory rate from PPG signals, the overall performance is still limited.

Possible reasons include:

- The BIDMC dataset contains only **53 patients**, which limits the diversity available for training.
- Respiratory rate shows relatively small variation for many patients, making it difficult for the model to learn robust patterns.
- Motion artifacts and physiological noise in PPG signals can affect prediction accuracy.
- A CNN captures local temporal features but may not fully model long-term dependencies present in respiratory patterns.
- The FFT-based label generation approach may introduce small errors in the ground-truth respiratory rate.

These factors contribute to the relatively low R² score despite achieving a reasonable MAE.

---

## Future Improvements

The following improvements can further enhance the model:

- CNN + BiLSTM architecture
- Transformer-based temporal modeling
- Attention Mechanism
- Multi-scale CNN blocks
- Larger and more diverse datasets
- Better respiratory rate label estimation techniques
- Advanced signal quality assessment before training
- Data augmentation for physiological signals

---

## Repository Structure

```
Respiratory-Rate-Estimation-from-PPG
│
├── 01_Data_Preprocessing.ipynb
├── 02_CNN_Training_and_Evaluation.ipynb
├── best_CNN.keras
├── requirements.txt
├── README.md
└── images/
```

---

## Requirements

```
numpy
scipy
tensorflow
scikit-learn
matplotlib
pandas
wfdb
```

Install using:

```bash
pip install -r requirements.txt
```

---



## Conclusion

This project demonstrates that respiratory rate can be estimated directly from PPG signals using deep learning without requiring a dedicated respiratory sensor during inference.

While the current CNN model achieves a mean absolute error of approximately **2.45 breaths/min**, there is still room for improvement in terms of generalization and robustness. Future work will focus on more advanced temporal deep learning architectures and improved preprocessing techniques to further enhance prediction accuracy.

---



