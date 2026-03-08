# Human Activity Recognition using Hidden Markov Models and Smartphone Sensors

This project implements a **Human Activity Recognition (HAR)** system using smartphone inertial sensor data and **Hidden Markov Models (HMMs)**. The system recognizes physical activities such as **walking, standing, jumping, and stillness** by analyzing accelerometer and gyroscope signals collected from a smartphone.

Human activities are inherently **temporal processes**, meaning observations evolve over time. Hidden Markov Models provide a probabilistic framework for modeling sequential data by representing observations as emissions generated from hidden states that evolve according to a Markov process.

The project demonstrates how sequential probabilistic models can be used to classify human activities from smartphone sensor data.

---

# Project Overview

The system follows a complete machine learning pipeline:

1. Sensor Data Preprocessing  
2. Exploratory Data Visualization  
3. Feature Engineering (Time and Frequency Domain)  
4. Sliding Window Segmentation  
5. Feature Normalization  
6. Hidden Markov Model Training using Baum–Welch  
7. Activity Prediction using Viterbi Decoding  
8. Model Evaluation on Unseen Data  

Two modeling approaches are implemented:

- **Standard Gaussian Hidden Markov Model**
- **HMM with State Persistence and Emission Noise Stabilization**

Both models are evaluated using **window-level predictions** and **sequence-level predictions**.

---

# Dataset Description

Sensor data was collected using a smartphone during four activities:

- Walking
- Standing
- Jumping
- Still

Each recording contains **three-axis accelerometer and three-axis gyroscope measurements**, capturing both linear acceleration and rotational motion.

The raw CSV sensor files are converted into structured dataframes and processed through several preprocessing steps:

- Timestamp normalization
- Sensor synchronization
- Axis renaming
- Metadata labeling

Each observation includes:

- `x_acc`, `y_acc`, `z_acc` (accelerometer axes)
- `x_gyro`, `y_gyro`, `z_gyro` (gyroscope axes)
- `seconds_elapsed`
- `activity`
- `participant`
- `recording_id`

---

# Data Visualization

Exploratory visualizations were created to inspect motion patterns for each activity.

Accelerometer and gyroscope signals were plotted across time for each axis.

| Activity | Sensor Pattern |
|--------|----------------|
Walking | Periodic oscillations |
Standing | Near-flat signal |
Jumping | Large acceleration spikes |
Still | Minimal variation |

These visual differences confirm that the sensor signals contain distinguishable patterns suitable for activity recognition.

---

# Feature Engineering

Raw sensor time-series data is converted into numerical features using **time-domain, magnitude, and frequency-domain features**.

## Time-Domain Features

Statistical features extracted for each sensor axis:

- Mean
- Standard deviation
- Minimum
- Maximum
- Root Mean Square (RMS)

These features capture signal intensity and variability.

---

## Magnitude Features

Movement magnitude is computed to capture overall motion independent of direction:

- Accelerometer magnitude
- Gyroscope magnitude

| Activity | Motion Magnitude |
|--------|----------------|
Still | Low |
Walking | Moderate |
Jumping | High |

---

## Frequency-Domain Features

The **Fast Fourier Transform (FFT)** is used to extract periodic motion characteristics.

Frequency features include:

- Spectral energy
- Dominant frequency
- Spectral entropy

These features help identify rhythmic activities such as walking.

---

# Sliding Window Segmentation

Continuous sensor data is segmented using a **sliding window approach**.

- Sampling rate: **100 Hz**
- Window size: **100 samples (~1 second)**
- Step size: **50 samples (50% overlap)**

This allows the system to capture short-term motion patterns while preserving temporal continuity.

---

# Feature Normalization

All extracted features are standardized using **Z-score normalization**.

This ensures that features with larger numerical ranges do not dominate the model training process and improves numerical stability.

---

# Hidden Markov Model Implementation

Hidden Markov Models are probabilistic graphical models designed for sequential data.

An HMM is defined by three parameters:

- **Initial state probabilities (π)**
- **Transition matrix (A)**
- **Emission distributions (B)**

In this project, emission probabilities are modeled using **multivariate Gaussian distributions**, resulting in a **Gaussian Hidden Markov Model**.

---

# Baum–Welch Training

The model parameters are estimated using the **Baum–Welch algorithm**, an Expectation–Maximization method for training HMMs.

Training involves two iterative steps:

### Expectation Step (E-Step)

The forward–backward algorithm computes the probability of each hidden state given the observed data.

### Maximization Step (M-Step)

Model parameters are updated to maximize the likelihood of the observed sequences.

Training continues until the **log-likelihood converges**, indicating stable model parameters.

---

# Model Variants

## Standard Hidden Markov Model

The baseline model trains a Gaussian HMM using Baum–Welch. Emission distributions are initialized using **K-means clustering** to provide better starting parameters.

---

## HMM with State Persistence and Emission Noise

A second model introduces stabilization techniques.

### State Persistence

Self-transition probabilities are increased to encourage hidden states to persist over time.

### Emission Noise

Small Gaussian noise is added to emission means to prevent states from collapsing into identical distributions.

---

# Prediction Methods

## Window-Level Prediction

Each feature window is classified independently using the **Viterbi algorithm** to determine the most likely hidden state sequence.

---

## Sequence-Level Prediction

Entire recordings are classified as sequences.

Predictions across all windows are aggregated using **majority voting**, improving stability and reducing the effect of isolated misclassifications.

---

# Results

| Model | Window-Level Accuracy | Sequence-Level Accuracy |
|------|------|------|
Standard HMM | 69.7% | 75.0% |
HMM with Stabilization | 57.6% | 62.5% |

Sequence-level prediction consistently improves performance by leveraging temporal dependencies in the data.

---

# Key Observations

- **Still activity is classified most accurately** due to minimal signal variation.
- **Standing is occasionally confused with walking** because of similar low-motion patterns.
- **Sequence-level predictions outperform window-level predictions**, highlighting the importance of temporal modeling.

---

# Conclusion

This project demonstrates a complete **Hidden Markov Model pipeline for human activity recognition using smartphone sensor data**. The system integrates sensor synchronization, feature engineering, HMM training using the Baum–Welch algorithm, and activity classification using Viterbi decoding.

The results show that modeling temporal dependencies through sequence-level prediction improves classification performance.

Future work could include:

- Collecting larger datasets
- Exploring additional features
- Comparing HMM performance with deep learning models for activity recognition

---

# References

Rabiner, L. R. (1989).  
*A tutorial on hidden Markov models and selected applications in speech recognition.*

Lara, O. D., & Labrador, M. A. (2013).  
*A survey on human activity recognition using wearable sensors.*

Kwapisz, J. R., Weiss, G. M., & Moore, S. A. (2011).  
*Activity recognition using cell phone accelerometers.*
