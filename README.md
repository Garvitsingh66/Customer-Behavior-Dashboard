<div align="center">

# Customer Behavior Analysis

<img src="Images/Customer Behavior.png" alt="Customer Behavior Dashboard" width="100%"/>

<br/>

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org/)
[![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](#)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](#)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white)](#)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Kaggle](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/)

**An end-to-end data analysis project exploring retail customer purchasing behavior using SQL for data extraction, Python for exploratory data analysis, and Power BI for final visualization.**

[Project Objective](#project-objective) · [Key Insights](#key-insights) · [SQL Analysis](#sql-analysis) · [Python EDA](#python-eda) · [Getting Started](#getting-started)

</div>

---

## Project Objective

This project performs a deep-dive analysis into retail customer behavior using a structured data pipeline — starting with SQL for data querying and aggregation, followed by Python for exploratory data analysis, pattern discovery, and visualization, and concluding with a Power BI dashboard for interactive reporting. The goal is to uncover actionable insights around customer spending habits, subscription adoption, demographic trends, category preferences, and shipping behavior across approximately 3,900 customers.

---

## At a Glance

<div align="center">

| Number of Customers | Avg Purchase Amount | Avg Review Rating | Subscribers | Non-Subscribers |
|:-:|:-:|:-:|:-:|:-:|
| **3,900** | **$59.76** | **3.75 / 5** | **27%** | **73%** |

</div>

---

## Key Insights

> **Spending**
> - Average purchase amount of **$59.76** reflects a stable mid-range retail segment
> - **Clothing dominates** both revenue and customer count across all categories
> - **Accessories** is the second strongest category, driven by frequent low-ticket purchases

> **Subscriptions**
> - Only **27% of customers subscribe** — 73% remain unsubscribed, representing a major conversion opportunity
> - Subscription campaigns targeted at Young Adults and high-frequency buyers could significantly lift recurring revenue

> **Demographics**
> - **Young Adults** are the highest revenue-generating age group and lead in total sales volume
> - **Middle-aged customers** form a strong and consistent second segment
> - **Senior customers** are underrepresented — either an untapped market or a reach challenge worth investigating

> **Satisfaction**
> - Average review rating of **3.75 / 5** suggests moderate satisfaction — room for improvement in product quality and delivery experience
> - Multiple shipping options (2-Day, Express, Free, Next Day Air) contribute to retention but may affect margin

---

## SQL Analysis

```sql
-- Total customers and average purchase amount
SELECT
    COUNT(Customer_ID)             AS Total_Customers,
    ROUND(AVG(Purchase_Amount), 2) AS Avg_Purchase_Amount,
    ROUND(AVG(Review_Rating), 2)   AS Avg_Review_Rating
FROM customers;

-- Revenue and customer count by category
SELECT
    Category,
    COUNT(Customer_ID)             AS Total_Customers,
    SUM(Purchase_Amount)           AS Total_Revenue,
    ROUND(AVG(Purchase_Amount), 2) AS Avg_Spend
FROM customers
GROUP BY Category
ORDER BY Total_Revenue DESC;

-- Subscription breakdown
SELECT
    Subscription_Status,
    COUNT(Customer_ID) AS Total,
    ROUND(COUNT(Customer_ID) * 100.0 / (SELECT COUNT(*) FROM customers), 2) AS Percentage
FROM customers
GROUP BY Subscription_Status;

-- Revenue by age group
SELECT
    CASE
        WHEN Age BETWEEN 18 AND 25 THEN 'Young Adult'
        WHEN Age BETWEEN 26 AND 45 THEN 'Middle-aged'
        WHEN Age BETWEEN 46 AND 60 THEN 'Adult'
        ELSE 'Senior'
    END AS Age_Group,
    COUNT(Customer_ID)   AS Total_Customers,
    SUM(Purchase_Amount) AS Total_Revenue
FROM customers
GROUP BY Age_Group
ORDER BY Total_Revenue DESC;

-- Top shipping preferences
SELECT
    Shipping_Type,
    COUNT(Customer_ID) AS Total_Orders
FROM customers
GROUP BY Shipping_Type
ORDER BY Total_Orders DESC;

-- Gender-wise spending
SELECT
    Gender,
    ROUND(AVG(Purchase_Amount), 2) AS Avg_Spend,
    SUM(Purchase_Amount)           AS Total_Revenue
FROM customers
GROUP BY Gender;
```

---

## Python EDA

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('customer_behavior.csv')

# Basic info
print(df.shape)
print(df.isnull().sum())
print(df.describe())

# Clean & standardize
df.drop_duplicates(inplace=True)
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')

# Age group segmentation
def age_group(age):
    if age <= 25:
        return 'Young Adult'
    elif age <= 45:
        return 'Middle-aged'
    elif age <= 60:
        return 'Adult'
    else:
        return 'Senior'

df['age_group'] = df['age'].apply(age_group)

# Purchase amount distribution
plt.figure(figsize=(8, 4))
sns.histplot(df['purchase_amount'], bins=30, kde=True, color='steelblue')
plt.title('Purchase Amount Distribution')
plt.xlabel('Purchase Amount ($)')
plt.tight_layout()
plt.show()

# Revenue by category
df.groupby('category')['purchase_amount'].sum().sort_values(ascending=False).plot(
    kind='bar', color='mediumpurple', figsize=(8, 4)
)
plt.title('Revenue by Category')
plt.ylabel('Total Revenue ($)')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Subscription breakdown
df['subscription_status'].value_counts().plot(
    kind='pie', autopct='%1.1f%%',
    colors=['#e74c3c', '#2ecc71'], figsize=(5, 5)
)
plt.title('Subscriber vs Non-Subscriber')
plt.ylabel('')
plt.show()

# Revenue by age group
df.groupby('age_group')['purchase_amount'].sum().sort_values().plot(
    kind='barh', color='slateblue', figsize=(8, 4)
)
plt.title('Revenue by Age Group')
plt.xlabel('Total Revenue ($)')
plt.tight_layout()
plt.show()

# Correlation heatmap
sns.heatmap(df[['age', 'purchase_amount', 'review_rating']].corr(),
            annot=True, cmap='coolwarm')
plt.title('Correlation Heatmap')
plt.show()
```

---

## Power BI Dashboard

The Power BI dashboard serves as the final presentation layer built on top of the SQL and Python analysis, featuring subscription breakdown, category revenue, age group performance, and interactive slicers for Gender, Category, Subscription Status, and Shipping Type.

---

## Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/Garvitsingh66/Customer-Behavior-Analysis.git
cd Customer-Behavior-Analysis

# 2. Install Python dependencies
pip install pandas numpy matplotlib seaborn

# 3. Run SQL queries
#    Open analysis.sql in MySQL Workbench or any SQL client

# 4. Run Python EDA
jupyter notebook eda.ipynb

# 5. Open Power BI dashboard
#    Launch Power BI Desktop and open Customer_Behavior_Dashboard.pbit
```

---

## Dataset

- **Source:** [Kaggle — Customer Shopping Behavior](https://www.kaggle.com)
- **Volume:** ~3,900 customer records
- **Key Fields:** `Customer_ID`, `Age`, `Gender`, `Category`, `Purchase_Amount`, `Review_Rating`, `Subscription_Status`, `Shipping_Type`

---

## Author

**Garvit Singh**
Aspiring Data Analyst | Python & SQL Enthusiast

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github&logoColor=white)](https://github.com/Garvitsingh66)

---

## Support

If you found this project useful, give it a star on GitHub!

[![Star on GitHub](https://img.shields.io/github/stars/Garvitsingh66/Customer-Behavior-Analysis?style=social)](https://github.com/Garvitsingh66/Customer-Behavior-Analysis)
