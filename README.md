# 🛒 Retail Customer Analytics & Cross-Sell Intelligence Engine

An end-to-end data engineering, machine learning, and business intelligence solution designed to segment retail customers and generate automated product recommendations. 

---

## 📸 Dashboard Preview

### Page 1: Customer Lifecycle & Segmentation
![Customer Segmentation](screenshots/page1_customer_segmentation.png)

### Page 2: Product Association & Cross-Sell Engine
![Cross-Sell Engine](screenshots/page2_cross_sell_engine.png)

---

## 📌 Executive Summary
* **Problem:** E-commerce platforms struggle with customer churn and untargeted marketing strategies, leading to lost revenue opportunities.
* **Solution:** Engineered a 2-stage platform combining **RFM Segmentation via K-Means Clustering** to isolate customer risk profiles with an **Association Rule Mining (FP-Growth)** cross-sell engine to surface automated product recommendations.
* **Business Value:** Identified **$2.10M in At-Risk revenue** for win-back campaigns and mapped **248 high-confidence cross-sell product pairs** to increase Average Order Value (AOV).

---

## 🛠️ Data Pipeline & Technical Architecture

`[ Raw Data (UCI) ]` ➡️ `[ Data Wrangling & Feature Engineering (Python/Pandas) ]` ➡️ `[ Log Normalization & K-Means Clustering (scikit-learn) ]` ➡️ `[ Association Rule Mining (Itertools/FP-Growth) ]` ➡️ `[ Star Schema Modeling & Interactive Dashboard (Power BI) ]`

### 1. Data Cleaning & Engineering
* Cleaned 500k+ transaction records by stripping missing `CustomerID`s, filtering administrative stock codes (e.g., `POST`, `D`, `M`), and removing cancelled transactions (`InvoiceNo` starting with 'C').
* Focused transaction scope on the UK region to maintain a homogeneous core market baseline.

### 2. Customer Segmentation (RFM + K-Means)
* Engineered **Recency**, **Frequency**, and **Monetary (RFM)** features at the customer level.
* Applied `np.log1p` transformation and `StandardScaler` to resolve high right-skewness before running **K-Means Clustering ($K=4$)**.
* Mapped behavioral segments: **Champions (VIP)**, **At Risk**, **New / Promising**, and **Lost / Hibernating**.

### 3. Cross-Sell Engine (Association Rule Mining)
* Implemented an optimized pair-itemset algorithm evaluating transactional co-occurrence.
* Configured thresholds: Support >= 1.5%, Confidence >= 10%, and Lift > 1.2.
* Generated **248 statistically significant association rules** exported directly to Power BI.

---

## 📊 Power BI Data Architecture & Star Schema
The Power BI model utilizes an optimized Star Schema centered around transactional facts:
* **`fact_transactions_clean`**: Transaction-level granularity (`Quantity`, `UnitPrice`, `TotalSpend`).
* **`dim_customer_rfm_clustered`**: Customer dimension holding RFM metrics, cluster IDs, and segment personas.
* **`dim_product_recommendations`**: Recommendation engine table linking target products (`Antecedent`) to recommended items (`Consequent`).

---

## 🗃️ Dataset Attribution
* **Dataset Name:** Online Retail Dataset
* **Source:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/online+retail)
* **License / Access:** Publicly available for research and educational purposes.
* **Dataset Description:** Contains 541,909 raw transactional records occurring between 01/12/2010 and 09/12/2011 for a UK-based non-store online retail business selling unique all-occasion gifts.

---

## 📁 Repository Structure
```text
├── data/               # Raw and processed CSV/XLSX data artifacts
├── notebooks/          # Clean, executed Jupyter Notebook with full ML pipeline
├── powerbi/            # Interactive Power BI (.pbix) dashboard file
├── screenshots/        # High-resolution dashboard visuals for documentation
└── README.md           # Project documentation and architectural overview