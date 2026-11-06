# NYC Yellow Taxi 2015–2016: from exploration to forecasting

An end-to-end analysis of 277 million NYC yellow taxi trips (TLC trip records, January 2015 – December 2016), carried out in five stages. Each stage is one notebook and builds on the previous one.

| Stage | Notebook | What it does |
|---|---|---|
| 1&nbsp;· Exploration | [01_data_exploration.ipynb](notebooks/01_data_exploration.ipynb) | Exploratory analysis of all 24 monthly files with DuckDB (no sampling): schema, data quality, distributions, outliers, time and location patterns. |
| 2&nbsp;· Feature engineering | [02_feature_engineering.ipynb](notebooks/02_feature_engineering.ipynb) | Cleaning decisions (delete as few rows as possible, flag the rest), feature derivation, encoding and scaling; output is a trip-level feature matrix. |
| 3&nbsp;· Statistical anomaly detection | [03_anomaly_detection.ipynb](notebooks/03_anomaly_detection.ipynb) | Statistical anomaly detection on the feature matrix: univariate and multivariate methods and time-based methods on the hourly series, as a baseline for ML methods. |
| 4&nbsp;· Model-based anomaly detection | [04_model_anomaly_detection.ipynb](notebooks/04_model_anomaly_detection.ipynb) | Model-based anomaly detection (Isolation Forest, LOF, One-Class SVM, XGBoost residual models, autoencoders) compared with the statistical baseline, using injected errors, a known real error and a tariff-based labelling rule. |
| 5&nbsp;· Forecasting | [05_time_series_forecasting.ipynb](notebooks/05_time_series_forecasting.ipynb) | Hourly trip count forecasting: seasonal baseline, LightGBM / XGBoost, LSTM and Chronos-2 (zero-shot and fine-tuned), time-based split, block bootstrap comparisons and error analysis. |

Inside the notebooks, earlier stages are referred to as Task 1–5.

## Forecasting result

Day-ahead forecast of the 24 hours of each day, test period July–December 2016 (WAPE %): baseline 7.73, LightGBM 5.95, LSTM (5 seeds) 5.94, Chronos-2 zero-shot 5.69. All three models beat the baseline; the differences between them were not statistically distinguishable on this test. Most of the remaining error sits on holidays and the days after them. Details, including 168-hour and 1-hour horizons and the 15 busiest zones, are in the notebook.

## Data and running the notebooks

The notebooks are stored with their outputs, so they can be read on GitHub without running anything. If GitHub does not render a large notebook, the same files open on [nbviewer](https://nbviewer.org/github/R7Murat/nyc-taxi-anomaly-forecasting/tree/main/notebooks/). The data is not part of this repository: the raw trip records are published by the NYC Taxi & Limousine Commission (TLC trip record data), and later notebooks read the feature matrix and saved model outputs produced by the earlier stages. Re-running them requires recreating those files locally.

Environment: Python 3.11, DuckDB for full-data aggregations, pandas, scikit-learn, LightGBM, XGBoost, PyTorch (CPU) and chronos-forecasting.
