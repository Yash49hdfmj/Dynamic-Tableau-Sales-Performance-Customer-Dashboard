# Sales & Customer Analytics Dashboards – Tableau Project (Multi-Year)

## 1. Overview

This project includes two advanced Tableau dashboards designed using 4–5 years of historical data. These dashboards enable dynamic analysis of sales performance and customer behavior through interactive filters, calculated fields, and clearly defined KPIs.

Ideal for business stakeholders, analysts, and technical interviews, this project demonstrates proficiency in KPI engineering, trend analysis, and dashboard interactivity.

---

## 2. Dashboards Included

### 2.1 Sales Dashboard

**Purpose**
Analyze revenue trends, profit margins, and product performance over multiple years, with context from previous year comparisons and category-level breakdowns.

**Key KPIs**

* **Total Sales**: Measures the total revenue generated in the selected year.
* **Total Profit**: Indicates the absolute profit (Sales - Costs) for the current year.
* **Total Quantity Sold**: Shows the number of units sold, useful for volume trend tracking.
* **% Difference in Sales vs PY**: Highlights year-over-year growth or decline in revenue.
* **% Difference in Profit vs PY**: Measures profitability change compared to last year.
* **% Difference in Quantity vs PY**: Tracks volume increase or decrease year over year.
* **Sales by Sub-Category**: Compares current and previous year sales per product line.
* **Profit by Sub-Category**: Identifies profitable and loss-making categories with color-coded bars.
* **Min/Max Sales**: Captures the best and worst-performing months dynamically.
* **KPI Averages**: Calculates rolling averages for Sales and Profit to analyze long-term stability.

**Insights**

* 2023 Total Sales: \$164.4M
* 2023 Total Profit: \$27.9M
* Quantity Sold: 12K units
* Year-over-Year Growth: +20.6% in Sales, +30.4% in Profit, +26.8% in Quantity
* High-profit categories: Phones, Chairs, Accessories
* Loss-making categories: Tables, Envelopes

---

### 2.2 Customer Dashboard

**Purpose**
Track customer growth, order activity, and identify high-value clients for retention strategies.

**Key KPIs**

* **Total Customers**: Count of distinct customers acquired in the selected year.
* **Total Orders**: Number of sales transactions placed.
* **Sales per Customer**: Average revenue generated per customer.
* **% Difference in Customers vs PY**: Growth or drop in customer base over time.
* **% Difference in Orders vs PY**: Measures customer engagement and transaction frequency.
* **Top 10 Customers by Profit**: Lists most valuable clients by profit contribution.
* **Last Order Date**: Shows customer recency (RFM analysis element).
* **Customer Order Distribution**: Histogram showing how many customers fall into various order volume buckets.

**Insights**

* 2023 Customers: 693
* 2023 Orders: 1,687
* Sales per Customer: \$237,261
* YoY Growth: +8.6% in Customers, +28.3% in Orders
* Key Customers: Tom Ashbrook, Justin Deggeller
* Most active segment: \~100 customers with 90+ orders

---

## 3. Filters and Interactivity

* **Select Year**: Dynamic parameter to view KPIs for any year from the dataset.
* **Region, State, City**: Location filters to drill down into geographic performance.
* **Category / Sub-Category**: Enables analysis by product hierarchy.

All visualizations auto-update based on selected filters and parameters.

---

## 4. Calculations and Logic

* **% Difference Calculations**: Custom fields comparing CY vs PY for each KPI.
* **Min/Max Detection**: Identifies extreme months using dynamic LOD expressions.
* **KPI Averages**: Calculated using moving average or fixed LODs for comparison with current values.
* **Above/Below Trend Visuals**: Sales and Profit trends are color-coded based on average thresholds.

---

## 5. Tools and Technologies

* Tableau Desktop
* Level of Detail (LOD) Calculations
* Parameter Controls and Quick Filters
* Time Series and Trend Analysis
* Interactive UX Design for Business Dashboards

---

## 6. Objective

The goal of this project is to create a highly interactive, multi-layered BI solution that allows business users to explore trends, compare metrics across years, and make data-informed decisions. This is ideal for interviews and showcases advanced Tableau capabilities in a real-world scenario.
