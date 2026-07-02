# Customer Segmentation using RFM Analysis & K-Means Clustering

Is project mein **Online Retail II** dataset use karke customers ko unke purchasing behavior ke basis par segments mein divide kiya gaya hai, using **RFM Analysis** (Recency, Frequency, Monetary) aur **K-Means Clustering**.

## Dataset

- File: `online_retail_II.csv.zip`
- Original rows: 1,067,371
- Columns: Invoice, StockCode, Description, Quantity, InvoiceDate, Price, Customer ID, Country

## Task Objective

Customers ko unke transaction history ke basis par meaningful segments (clusters) mein group karna — taake business alag-alag customer types (jaise high-value loyal customers vs one-time low spenders) ko identify karke unke liye targeted marketing strategy bana sake.

## Approach

1. **Data Loading & Exploration**
   - Zipped CSV load kiya
   - `.info()`, `.describe()`, aur missing values check kiye

2. **Data Cleaning**
   - Missing `Customer ID` wale rows drop kiye
   - Negative/zero `Quantity` aur `Price` wale rows remove kiye (returns/invalid entries)
   - `InvoiceDate` ko proper datetime format mein convert kiya
   - Result: 1,067,371 → 805,549 rows

3. **RFM Feature Engineering**
   - `TotalPrice` = Quantity × Price
   - Snapshot date = dataset ki last invoice date + 1 din
   - Per customer aggregate kiya:
     - **Recency** — last purchase se kitne din guzray
     - **Frequency** — unique invoices ki count
     - **Monetary** — total kharch ki hui amount

4. **Data Transformation**
   - Skewed distribution ko normalize karne ke liye Recency, Frequency, Monetary par **log transformation** apply ki
   - Phir **StandardScaler** se features ko scale kiya

5. **Optimal Cluster Count**
   - **Elbow Method** use kiya (K=1 se 10 tak test kiya) optimal number of clusters dhoondne ke liye

6. **K-Means Clustering**
   - K=3 clusters ke sath K-Means model fit kiya
   - Har customer ko ek cluster label assign hua

7. **Visualization**
   - **PCA** (2 components) se dimensionality reduce ki
   - Clusters ko 2D scatter plot mein visualize kiya

## Results and Findings

3 customer segments identify hue, jinke RFM averages kuch is tarah hain:

| Cluster | Recency (avg days) | Frequency (avg orders) | Monetary (avg spend) | Segment Type |
|---|---|---|---|---|
| **2** | 27.4 | 18.1 | **$10,229.90** | Best / High-Value Customers — recent, frequent, sab se zyada kharch karne wale |
| **1** | 141.0 | 4.4 | $1,534.07 | At-Risk / Mid-Value Customers — moderate activity, kuch waqt se inactive |
| **0** | 366.3 | 1.4 | $348.23 | Lost / Low-Value Customers — bohat purana purchase, kam frequency aur spend |

**Key Insight:** Cluster 2 sab se qeemti segment hai (recent + frequent + high spending) — inhe retain karna business ke liye sab se important hai. Cluster 0 ke customers churn ho chuke lagte hain aur reactivation campaigns ki zaroorat hai. PCA visualization ne teeno clusters ko clearly separate dikhaya.

## Requirements

```
pandas
numpy
scikit-learn
matplotlib
seaborn
```

## Note

Dataset zip format mein hai (`online_retail_II.csv.zip`), notebook ise directly `pd.read_csv(..., compression='zip')` se load karta hai — extract karne ki zaroorat nahi.
