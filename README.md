# Cybersecurity Alert Prioritization and False Positive Detection

This project simulates a real-world machine learning pipeline to process, enrich, prioritize, and classify cybersecurity alerts. The work includes data cleaning, enrichment from external sources, predictive modeling, and final alert scoring.

## What the Project Does

The project is divided into several parts:

1. **Data understanding and cleaning**
   - Explanation of each feature
   - Handling of missing values and unreliable data (e.g., removing "Rustscan" alerts)

2. **Contextual enrichment**
   - Using the NVD API to retrieve official CVSS scores based on `cve_id`
   - Replacing synthetic CVSS scores with official ones when available

3. **Alert prioritization (regression modeling)**
   - Training models to predict a custom severity score (`priority_score`) for each alert
   - Models used: Linear Regression, Random Forest Regressor, XGBoost Regressor

4. **False positive detection (classification modeling)**
   - Training models to predict whether an alert is a false positive
   - Models used: Logistic Regression, Random Forest Classifier, XGBoost Classifier
   - Instead of hard classification, the model predicts a probability (`false_positive_score`)

5. **Final outputs**
   - Each alert receives two scores:
     - `priority_score`: predicted severity level
     - `false_positive_score`: probability that the alert is a false positive

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

Python version required: 3.7+.

### 2. Prepare the Dataset

Place the provided dataset (`security_alerts_dataset.csv`) locally. Update the data loading line if needed:

```python
df = pd.read_csv("your/local/path/security_alerts_dataset.csv")
```

- If you use Google Colab, upload the file manually.
- If you use Jupyter or VS Code, adapt the path manually.

### 3. How to Understand the Code

The notebook follows a standard machine learning workflow:

- Step 1: Load and explore the data
- Step 2: Clean and enrich the data
- Step 3: Train models for regression (priority scoring)
- Step 4: Train models for classification (false positive detection)
- Step 5: Predict scores for all alerts
- Step 6: Visualize results

Every step in the notebook is documented. You can run the notebook sequentially from top to bottom without modification.

## Important Notes

- No hyperparameter tuning was done because the dataset is synthetic and does not reflect real-world behavior.
- CVSS scores in the original dataset were not reliable. Official scores were retrieved when possible.
- Rustscan alerts were excluded because they contained missing critical information.
- Model performance is limited due to the nature of the synthetic data.
- Final scores (`priority_score` and `false_positive_score`) simulate a real-world prioritization and triage system.

## Final Deliverables

- Each alert has a `priority_score` estimating its severity.
- Each alert has a `false_positive_score` estimating the probability of being irrelevant.
- The project is fully reproducible and well-documented.