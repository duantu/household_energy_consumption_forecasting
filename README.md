# Household Energy Consumption Forecasting

**Authors:** Andres Barei, Cisil Karaguzel, Yang Li, Olti Myrtaj, Duan Tu

---

## Executive Summary

This project investigates the use of deep learning models to forecast household power consumption in the United Kingdom (UK). With shifting energy consumption patterns, population growth, and increasing reliance on sustainable energy, accurate demand forecasting is becoming essential.

Accurate energy consumption predictions can enable:
- Cost savings for consumers and providers
- Improved power grid planning and infrastructure
- Easier integration of renewable energy sources
- Progress toward national policy goals such as the **UK Net Zero 2050 target**

**Primary stakeholders**:
- **UK energy providers** (e.g., UK National Grid, British Gas)
- **Policy makers & regulators** (e.g., Department for Energy Security and Net Zero)
- **Smart home technology developers** (e.g., Google Nest, Samsung SmartThings)

We explore whether training deep learning models on representative households can produce accurate short-term forecasts, and compare performance against baseline statistical models (ARIMA).

---

## Data

**Source:** [Low Carbon London Project](https://huggingface.co/datasets/OpenSynth/TUDelft-Electricity-Consumption-1.0)
- **Original dataset:** 5,567 households in London, November 2011 – February 2014  
- **Resolution:** 30-minute intervals (48 time steps/day)  
- **Size:** ~10 GB

**Preprocessing:**
- Selected 919 households with complete data over a 13-month period (Nov 1, 2012 – Nov 30, 2013)
- Final dataset: ~900 MB
- Clustering to select representative households:
  1. **Dimensionality reduction** using Piecewise Aggregation Approximation (PAA)
  2. **k-Means clustering** in reduced space
  3. Selecting households closest to each centroid for training

---

## Baseline Models

### Naive Forecasting
- **Short-term (1 day):** Predict next day’s consumption using previous day’s value
- **Long-term (1–2 months):** Predict using the average of the last 14 days

### Seasonal ARIMA (SARIMA)
- Seasonality: 7 days
- Parameters chosen from ACF/PACF analysis
- Works well for short-term forecasting, but slow to train and tune for long-term

---

## Deep Learning Models

We implemented and compared **two sequence-to-sequence LSTM forecasting models**:
1. **Model 1:** Trained on the first five households (after separating test set)
2. **Model 2:** Trained on five representative households selected via clustering

**Architecture:**
- Input: 336 time steps (7 days), 1 consumption feature, 6 engineered time features
- Encoder: LSTM (64 units)
- Repeat Vector
- Decoder: LSTM (64 units)
- Output: 48 time steps (next day’s 30-minute intervals)

---

## Repository Structure
project-root/
│
├── Data/ # Raw & preprocessed datasets
├── EDA/ # Exploratory data analysis, preprocessing and feature engineering
├── Models/ # Saved models & training scripts
├── trial_models/ # Trial models developed during model exploration
└── README.md # Project documentation
