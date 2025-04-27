# Security Alerts Processing and False Positive Prediction

## Project Overview

This project is part of a technical recruitment challenge.  
The goal is to process a dataset of security alerts and develop a model that can predict whether an alert is a false positive.

The steps completed in this project are:

1. **Data Cleaning**:  
   - Removal of duplicates
   - Correction of formatting issues
   - Structuring and organizing the data

2. **Contextualization**:  
   - Enrichment of alerts with additional information (threat intelligence, vulnerabilities history, etc.)

3. **False Positive Identification**:  
   - Development of a predictive model using **XGBoost**.
   - The model outputs a **score between 0 and 1** representing the probability that an alert is a **false positive**.

4. **Classification and Prioritization**:  
   - Alerts are scored and prioritized based on their criticality and impact.

5. **Visualization**:  
   - A dashboard or synthetic report is built to facilitate analysis and decision-making.

---

## How to Execute the Code

1. **Clone the repository**:
   ```bash
   git clone <repository_url>
   cd <repository_directory>
