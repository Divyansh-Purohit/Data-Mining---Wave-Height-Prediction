# EMM on Ocean Wave Height Prediction

This project explores ocean wave height behavior using Exceptional Model Mining (EMM) and builds a predictive framework on top of buoy-level time-series data. The goal is to uncover subgroups with exceptional characteristics in wave patterns and evaluate how these patterns influence predictive performance.

Why this matters: Understanding abnormal or exceptional wave-height behavior is crucial for maritime safety, coastal planning, and marine energy applications. EMM acts as a “pattern detective” to surface interesting subpopulations before traditional modeling even begins.

## Project Overview

This repository focuses on two major tasks:

1. Exploratory Subgroup Discovery with EMM
	•	Uses Exceptional Model Mining (EMM) to uncover data regions where wave behavior deviates significantly from the global norm.
	•	Evaluates exceptional subgroups based on model quality, error divergence, and pattern distinctiveness.
	•	Helps identify:
	•	High-impact weather windows
	•	Seasonal anomalies
	•	Station-specific differences
	•	Conditions that drive rare wave-height spikes

2. Predictive Modeling for Wave Height Estimation
	•	Builds regression-based models on selected buoy stations.
	•	Compares performance before and after subgroup-aware transformations.
	•	Experiments with:
	•	Time-series features such as wave period, wind direction, pressure, and timestamps.

## Dataset

Data from the National Data Buoy Center (NDBC) for buoy 51101 is used.

Stations Used
	•	One or two stations selected based on:
	•	Data completeness
	•	Geographic relevance
	•	Distinct wave regimes

Data Includes:
	•	Significant wave height (primary target)
	•	Swell height & period
	•	Wind speed, gust, and direction
	•	Atmospheric pressure
	•	Timestamped readings (hourly)

The raw data is cleaned, resampled, and transformed before feeding into EMM and ML models.

## Techniques Used

Exceptional Model Mining (EMM)
	•	Subgroup discovery using quality measures like:
  	•	Model error deviation
  	•	Explained variance
  	•	Mean target deviation

  
  •	Implemented via Python:
  	•	Custom EMM routines
  	•	Rule-based subgroup definitions
  	•	Quality scoring and ranking

Machine Learning & Time-Series Prediction
	•	Regression modeling for wave height prediction.
	•	Feature engineering:
  	•	Lag features
  	•	Rolling windows
  	•	Harmonic time components
  	•	Wind–wave interaction variables

##  How to Run the Project

1. Install dependencies
2. Download buoy data
3. Run EMM analysis

## Output

You’ll find top-ranked exceptional subgroups.

## Contributions

If you want to extend this project:
	•	Add more buoy stations
	•	Test different EMM quality measures
	•	Use LSTMs or hybrid time-series DL models
