# Dataset and experiment provenance

This GitHub project is the **later, separately specified UCI full-dataset experiment**, not a published-paper replication.

- **Dataset:** UCI Electricity Load Diagrams 2011–2014, raw `LD2011_2014.txt`, 370 consumer columns. Source: https://doi.org/10.24432/C58C86
- **Study target:** hourly mean of the sum of all 370 consumer columns at each of the four quarter-hour readings.
- **Recorded original execution:** `notebooks/uci_forecasting_as_executed.ipynb`, transferred byte-for-byte from the Colab `.ipynb` provided by the author. SHA-256: `b6f4d9d49c52ea201f612f8fa3c968d293b1849333faad55a15ee4c284a17db7`
- **Results:** extracted from the notebook's stored Colab output text; model training was not independently rerun while assembling this repository.
- **Paper distinction:** Kanatnur, A. & Ayzeren, Y. B. (2026), *Data Science-Based Modeling for Electricity Consumption Forecasting* (DOI: https://doi.org/10.65247/3136-8042-2026-3.03) describes the UCI dataset but publishes different scores (XGBoost RMSE 155.80, LSTM RMSE 148.70). The exact original published-experiment code/data lineage is not verified here. Therefore, the notebook and test scores in this project **must not be presented as the original published experiment or a verified reproduction of it**.

The researcher should confirm the paper's code/data provenance with the coauthor if claiming computational reproducibility of the published work.
