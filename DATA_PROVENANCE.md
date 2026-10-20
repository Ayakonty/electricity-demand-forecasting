# Dataset and experiment provenance

This repository documents a standalone electricity load forecasting experiment conducted using the UCI Electricity Load Diagrams 2011–2014 dataset.

- **Dataset:** UCI Electricity Load Diagrams 2011–2014, raw `LD2011_2014.txt`, containing 370 consumer columns. Source: https://doi.org/10.24432/C58C86
- **Study target:** Hourly mean of the total load across all 370 consumers, calculated from quarter-hourly observations.
- **Recorded execution:** `uci_forecasting_as_executed.ipynb`, preserved from the author's Google Colab notebook. SHA-256: `b6f4d9d49c52ea201f612f8fa3c968d293b1849333faad55a15ee4c284a17db7`
- **Results:** The reported performance metrics were extracted from the notebook's saved execution outputs.
- **Verification status:** Model training was not independently rerun when preparing this repository. Exact results may vary across software versions, hardware, and nondeterministic training operations.
- **Research scope:** Findings are specific to this dataset, forecasting target, train-validation-test split, and model configurations. They are not claimed as a reproduction or verification of results from other experiments.

The notebook, saved results, and methodological limitations are provided for transparency and further examination.
