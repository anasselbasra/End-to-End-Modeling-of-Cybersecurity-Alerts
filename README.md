# Cybersecurity Alert Prioritization and False Positive Detection

This project simulates a real-world machine learning pipeline to process, enrich, prioritize, and classify cybersecurity alerts.  
The work includes data cleaning, enrichment from external sources, predictive modeling, and final alert scoring.

---

## What the Project Does

The project is divided into several parts:

1. **Data understanding and cleaning**  
   - Explanation of each feature.
   - Handling of missing values and unreliable data (e.g., removing "Rustscan" alerts).

2. **Contextual enrichment**  
   - Using the NVD API to retrieve official CVSS scores based on `cve_id`.
   - Replacing synthetic CVSS scores with official ones when available.

3. **Alert prioritization (regression modeling)**  
   - Training models to predict a custom severity score (`priority_score`) for each alert.
   - Models used: Linear Regression, Random Forest Regressor, XGBoost Regressor.

4. **False positive detection (classification modeling)**  
   - Training models to predict whether an alert is a false positive.
   - Models used: Logistic Regression, Random Forest Classifier, XGBoost Classifier.
   - Instead of hard classification, the model predicts a probability (`false_positive_score`).

5. **Final outputs**  
   - Each alert receives two scores:
     - `priority_score`: predicted severity level
     - `false_positive_score`: probability that the alert is a false positive.

---

## How to Set Up and Execute the Project

### 1. Install Dependencies

Install all required libraries using the `requirements.txt` file:

```bash
pip install -r requirements.txt
```

Libraries needed include:
- pandas
- numpy
- scikit-learn
- xgboost
- matplotlib
- tqdm
- requests

Python version required: **3.7+**.  
Check your Python version with:

```bash
python --version
```

---

### 2. Prepare the Dataset

- Place the provided dataset (`security_alerts_dataset.csv`) locally.
- Update the data loading line if needed:

```python
df = pd.read_csv("your/local/path/security_alerts_dataset.csv")
```

---

### 3. Important Note About CVSS Enrichment (NVD API)

When enriching CVSS scores from the NVD API:

- The process can take **40 minutes or more** if you pull all CVSS scores manually using the API.
- To avoid this, you can directly upload and load the provided file `cvss_results.pkl`, which contains all the retrieved scores:

```python
import pickle
with open("cvss_results.pkl", "rb") as f:
    results = pickle.load(f)
```

- Otherwise, if you still want to retrieve the CVSS scores yourself, **you must use an NVD API key**.

---

### 4. About the API Key

- An API key is valid for **7 days** after registration.
- You can request an API key from the NVD Developer Portal:  
  [NVD API Registration](https://nvd.nist.gov/developers/request-an-api-key)
- The API key must be inserted in the code when querying CVSS:

```python
cvss = get_cvss_from_nvd(cve_id, api_key="your-api-key-here")
```

- If you use an API key:
  - You are allowed **100 requests per minute** → about **1 request every 0.6 seconds**.

- If you do NOT use an API key:
  - You are limited to **5 requests per 30 seconds** → about **1 request every 6 seconds**.

Thus, if you do not use an API key, **you must change the sleep time** when looping over CVEs:

```python
for cve_id in tqdm(cve_ids_missing_cvss, desc="Fetching CVSS from NVD"):
    cvss = get_cvss_from_nvd(cve_id, api_key)
    results[cve_id] = cvss
    time.sleep(6)  # 6 seconds without an API key
```

instead of:

```python
time.sleep(0.6)  # 0.6 seconds with an API key
```

⚠️ If you forget to adjust the sleep time, you will be blocked by the NVD servers.

---

### 5. How to Understand the Code

The notebook follows a standard machine learning workflow:

- Load and explore the data.
- Clean and enrich the data.
- Train models for regression (priority scoring).
- Train models for classification (false positive detection).
- Predict scores for all alerts.
- Visualize results.

Each step is clearly commented.

---

## Final Deliverables

- Each alert has a `priority_score` estimating its severity.
- Each alert has a `false_positive_score` estimating the probability of being irrelevant.
- The project is fully reproducible and documented.
