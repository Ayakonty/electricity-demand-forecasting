# Electricity Load Forecasting with XGBoost and LSTM

**Ayakoz Kanatnur | AI / Data Science research portfolio**

An executed Google Colab experiment comparing **XGBoost**, **LSTM**, and **seasonal naive baselines** on electricity consumption data from the [UCI Electricity Load Diagrams 2011–2014](https://archive.ics.uci.edu/dataset/321/electricityloaddiagrams20112014) dataset.

> **Scope:** This repository documents a **separate, subsequent UCI experiment**. It is **not** the original training code for, or an exact numerical reproduction of, the 2026 journal article *Data Science-Based Modeling for Electricity Consumption Forecasting*. The paper's reported model scores differ from the results below.

## Dataset and research question

- **Source:** Trindade, A. (2015). *ElectricityLoadDiagrams20112014*. UCI Machine Learning Repository. [DOI: 10.24432/C58C86](https://doi.org/10.24432/C58C86).
- **Source file:** `LD2011_2014.txt`, downloaded from the UCI archive by the notebook. Raw dataset is **not included** here due to size.
- **Observed source dimensions:** 140,256 timestamps and 370 consumers, sampled at 15-minute intervals.
- **Forecast target:** total load across all 370 consumer columns, computed at each source timestamp and then **hourly averaged**. This is **not** a single-consumer forecast.
- **Timestamp convention:** subtract 15 minutes before hourly resampling, treating the source timestamp as the end of the 15-minute measurement period. After resampling: 35,064 hourly observations over 2011–2014.
- **Forecasting task:** rolling **one-hour-ahead** load forecasting; forecasts may use previously observed hourly loads, including earlier hours during the held-out test year.

## Model design

| Phase | Time period | Role |
| --- | --- | --- |
| Initial training | 2011–2012 | Fit candidate models; fit validation-stage scaler |
| Validation | 2013 | Assess models and choose the LSTM training schedule |
| Final training | 2011–2013 | Refit models, including final-training-only scaler |
| Held-out testing | 2014 | Evaluate results on 8,760 hourly targets |

- **Seasonal naive:** use the observed load from 24 or 168 hours earlier.
- **XGBoost:** four past-load lags (1, 24, 48, 168 hours), shifted rolling mean/std features, and calendar features. Training/validation was 2011–2012 / 2013, followed by a final 2011–2013 refit.
- **LSTM:** a 168-hour past-load sequence with 32 LSTM units and two Dense layers. The final model uses 49 epochs and a learning-rate schedule informed by the **2013 validation** run; final scaler is fit only on 2011–2013.
- **Evaluation:** RMSE and MAPE. RMSE uses the numerical scale of the aggregated target; no claim is made about a particular physical unit here.

The two learned models use **different inputs** (XGBoost has engineered calendar/lag features; LSTM uses past-load sequences). The comparison is of these specific end-to-end approaches, not isolated model architectures.

## Held-out test results — 2014

These numbers are **taken from the saved outputs of the provided executed Colab notebook**, not independently rerun by the repository packager.

| Model | RMSE ↓ | MAPE (%) ↓ |
| --- | ---: | ---: |
| **XGBoost** | **5,906.86** | **1.91** |
| LSTM | 7,083.74 | 2.36 |
| Naive-24 | 13,555.31 | 3.68 |
| Naive-168 | 20,769.35 | 5.13 |

![RMSE comparison](figures/final_test_rmse.png)

![MAPE comparison](figures/final_test_mape.png)

XGBoost's test RMSE is approximately **16.6% lower** than the LSTM result in this run. This does **not** establish that XGBoost is universally superior. One LSTM seed/run is insufficient to assess uncertainty or robustness.

## Validation results — 2013

| Model | RMSE ↓ | MAPE (%) ↓ |
| --- | ---: | ---: |
| XGBoost | 7,195.67 | 2.31 |
| LSTM (validation-selected checkpoint) | 9,721.07 | 3.54 |
| Naive-24 | 13,459.18 | 3.84 |
| Naive-168 | 19,947.32 | 5.24 |

The LSTM validation run continued to 50 epochs; its best checkpoint was at epoch **49**. The final model was **trained from scratch** for 49 epochs on the expanded 2011–2013 data with a fixed learning-rate schedule.

## Files

```text
notebooks/uci_forecasting_as_executed.ipynb   # Original exported Colab file, unchanged
results/final_test_2014.csv                # Extracted saved-output scores
results/validation_2013.csv                # Extracted saved-output scores
figures/final_test_rmse.png                # Visualization of saved-output scores
figures/final_test_mape.png                # Visualization of saved-output scores
DATA_PROVENANCE.md                         # Source and publication distinction
requirements.txt                           # Dependency families
.gitignore                                 # Files excluded from Git
```

## Running this notebook in Google Colab

1. Open `notebooks/uci_forecasting_as_executed.ipynb` in [Google Colab](https://colab.research.google.com/).
2. Use **Runtime → Run all**. The first cell downloads approximately 249 MB of source data from the UCI archive and unzips it; enough disk space and memory are required.
3. If Google Colab does not already provide dependencies, install those listed in `requirements.txt`.
4. Compare new metrics with `results/`. **Exact LSTM results can vary** with library/hardware versions and nondeterministic computations.

The notebook is preserved **as executed**, including training logs and cell outputs. The source file has not been edited to alter model scores. A notebook with stored outputs is evidence of a recorded execution, **not a substitute for independent reproducibility verification**.

## Methodological boundaries

- Past **test-period observations** are available to each next-hour prediction. Do not describe results as 24-hour-ahead/long-horizon forecasts.
- Model selection used 2013 validation, rather than the 2014 test year.
- The code predicts the aggregate load, whose numeric scale differs strongly from a per-consumer target.
- The notebook contains one final LSTM run; no multi-seed study, confidence interval, or forward-deployment test was conducted.
- See `DATA_PROVENANCE.md` for how this work differs from a related publication.
