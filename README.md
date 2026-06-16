<div align="center">

# Customer Behavior & Purchase Insights Dashboard

<img src="Images/Customer Behavior.png" alt="Customer Behavior Dashboard" width="100%"/>

<br/>

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](#)
[![Kaggle](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/)

**An interactive Power BI dashboard analyzing customer purchasing behavior, subscription trends, revenue by category, and demographic insights across 3,900 retail customers.**

[Key Insights](#key-insights) · [Tech Stack](#tech-stack) · [DAX Measures](#dax-measures) · [Getting Started](#getting-started)

</div>

---

## Project Objective

This project aims to understand retail customer behavior by building an interactive Power BI dashboard that reveals patterns in purchasing, subscription adoption, category preferences, and demographic segmentation. By combining DAX-powered KPIs with dynamic slicers for gender, category, subscription status, and shipping type, the dashboard enables stakeholders to drill into the data and make informed decisions around customer engagement, product strategy, and revenue growth.

---

## At a Glance

<div align="center">

| Number of Customers | Avg Purchase Amount | Avg Review Rating | Subscribers | Non-Subscribers |
|:-:|:-:|:-:|:-:|:-:|
| **3,900** | **$59.76** | **3.75 / 5** | **27%** | **73%** |

</div>

---

## Dashboard Overview

| Visual | Description |
|--------|-------------|
| **Percentage of Subscribers** | Donut chart showing 27% subscribed vs 73% non-subscribed customers |
| **Sales by Revenue** | Bar chart comparing total purchase amount across Clothing, Accessories, Footwear, and Outerwear |
| **Sales by Category** | Customer count distribution across all product categories |
| **Revenue by Age Group** | Horizontal bar showing Young Adults leading revenue, followed by Middle-aged and Adults |
| **Sales by Age Group** | Customer volume comparison across Young Adult, Middle-aged, and Senior segments |
| **Slicers** | Interactive filters for Subscription Status, Gender, Category, and Shipping Type |

---

## Key Insights

### Purchase Behavior
> - Average purchase amount of **$59.76** indicates a mid-range retail segment with consistent spending
> - **Clothing leads in both revenue and customer count**, making it the most dominant category across the board
> - **Accessories** ranks second in sales volume, suggesting strong demand for add-on purchases

### Subscription Trends
> - Only **27% of customers are subscribers** — a significant growth opportunity given that 73% remain unsubscribed
> - Targeted subscription campaigns toward high-spending segments could meaningfully improve retention and recurring revenue

### Demographics
> - **Young Adults generate the highest revenue** and account for the most sales volume across age groups
> - **Middle-aged customers** are a close second in both revenue and count — a valuable and stable segment
> - **Senior customers** represent the smallest segment, suggesting either low penetration or an untapped market

### Ratings & Shipping
> - Average review rating of **3.75 out of 5** reflects moderate satisfaction — room for improvement in product quality or delivery experience
> - Shipping options include **2-Day, Express, Free Shipping, and Next Day Air** — offering flexibility that likely contributes to customer retention

---

## Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Visualization | **Power BI Desktop** | Dashboard design, slicers, interactive visuals |
| Calculation | **DAX** | KPIs — Avg Purchase, Subscriber %, Customer Count |
| Data Source | **Kaggle** | Raw retail customer behavior dataset |

---

## DAX Measures

```dax
-- Average Purchase Amount
Avg Purchase Amount = AVERAGE('Customers'[Purchase_Amount])

-- Average Review Rating
Avg Review Rating = AVERAGE('Customers'[Review_Rating])

-- Total Customers
Total Customers = COUNTROWS('Customers')

-- Subscriber Percentage
Subscriber % =
DIVIDE(
    COUNTROWS(FILTER('Customers', 'Customers'[Subscription_Status] = "Yes")),
    COUNTROWS('Customers')
) * 100

-- Revenue by Category
Revenue by Category = SUM('Customers'[Purchase_Amount])
```

---

## Project Structure
