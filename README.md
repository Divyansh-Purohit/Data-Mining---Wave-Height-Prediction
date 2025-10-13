# Data-Mining---Wave-Height-Prediction
Group Project for EMM Group 3 
Dataset: NDBC Station 51101 from 2013 to 2017

Overview

This project predicts ocean wave height (WVHT) using historical buoy data and deep learning.
The core model is an LSTM-based time series forecaster, with SCAPE (Subgroup Characterization for Performance Explanation) applied afterward to analyze where the model performs exceptionally well—or poorly.

This helps not only with accurate forecasting but also with understanding model behavior under different ocean conditions.

Features used
Feature	Description	Units:
MM	Month (normalized)	—
hh	Hour (normalized)	—
WDIR	Wind Direction	Degrees (°)
WSPD	Wind Speed	m/s
GST	Wind Gust Speed	m/s
DPD	Dominant Wave Period	s
APD	Average Wave Period	s
MWD	Mean Wave Direction	Degrees (°)
PRES	Atmospheric Pressure	hPa
ATMP	Air Temperature	°C
WTMP	Water Temperature	°C
Temp_diff	WTMP - ATMP	°C

Target:
WVHT — Significant Wave Height (m)

Preprocessing
Missing Values
Filled using median imputation.

Scaling
All features and target scaled using scikit-learn MinMaxScaler.
Scaling was done after splitting to prevent data leakage.
Train-Test Split
Using train_test_split from scikit-learn.
Validation set is carved from training data during model training.

Sequencing
Input data transformed into overlapping sequences using Keras timeseries_dataset_from_array.

Model Architecture
Model: LSTM-based Sequential Network
Framework: TensorFlow / Keras

Optimizer: Adam
Loss: MSE
Metrics: MAE, RMSE, R² (computed post-prediction)
