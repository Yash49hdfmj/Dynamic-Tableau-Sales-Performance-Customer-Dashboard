# Sales & Customer Analytics Dashboards – Tableau Project

## Project Overview
![sales dashboard](https://github.com/Yash49hdfmj/Dynamic-Tableau-Sales-Performance-Customer-Dashboard/blob/main/datasets/sales%20dashboard.png)
![customer dashboard](https://github.com/Yash49hdfmj/Dynamic-Tableau-Sales-Performance-Customer-Dashboard/blob/main/datasets/customer%20dashboard.png)

### **Situation**
As an MSc Data Science student preparing for Business Analyst roles, I identified the need to demonstrate end-to-end business intelligence capabilities. The challenge was to create executive-level dashboards that could analyze multi-year sales performance and customer behavior for retail business decision-making, while showcasing advanced Tableau skills including LOD expressions, parameter-driven interactivity, and KPI engineering.

### **Task**
My objective was to build two comprehensive, interactive Tableau dashboards that would:
- **Track sales performance** with year-over-year comparisons across 4-5 years of historical data
- **Identify customer behavior patterns** and high-value clients for retention strategies  
- **Enable dynamic filtering** across time, geography, and product dimensions
- **Provide actionable insights** through calculated KPIs, trend analysis, and visual storytelling
- **Demonstrate technical proficiency** in dimensional modeling, LOD calculations, and dashboard design

---

## 🗂️ Data Architecture & Modeling

### **Action: Data Structure Design**

I designed a **star schema** data model following dimensional modeling best practices:

#### **Fact Table: Orders.csv (9,996 rows)**
- **Grain**: One row per product per order (transaction-level detail)
- **Measures**: Sales ($), Profit ($), Quantity (units), Discount (%)
- **Dimensions (Foreign Keys)**: Customer ID, Product ID, Postal Code, Order Date
- **Time Period**: 4-5 years of historical transactional data (2020-2023)
- **Business Context**: Contains all measurable business events - revenue, costs, units sold

**Sample Data:**
```
Row ID | Order ID | Order Date | Customer ID | Product ID | Sales | Quantity | Discount | Profit
1 | CA-2022-152156 | 08/11/2022 | CG-12520 | FUR-BO-10001798 | 261.96 | 2 | 0 | 41.91
4 | US-2021-108966 | 11/10/2021 | SO-20335 | FUR-TA-10000577 | 957.58 | 5 | 0.45 | -383.03
```

#### **Dimension Tables**

| **Table** | **Rows** | **Type** | **Primary Key** | **Attributes** | **Business Purpose** |
|-----------|----------|----------|----------------|----------------|---------------------|
| **Products.csv** | 1,896 | Dimension | Product ID | Category, Sub-Category, Product Name | Enables product hierarchy analysis (Category → Sub-Category → Product) for performance tracking |
| **Location.csv** | 634 | Dimension | Postal Code | City, State, Region, Country | Enables geographic drill-down and regional comparison (Region → State → City) |
| **Customers.csv** | 795 | Dimension | Customer ID | Customer Name | Enables customer-level analysis, retention tracking, RFM segmentation |

**Product Hierarchy Example:**
```
Technology → Accessories → Logitech Media Keyboard K200
Furniture → Chairs → Global Deluxe High-Back Office Chair
Office Supplies → Binders → Wilson Jones Legal Size Ring Binders
```

**Geographic Hierarchy Example:**
```
Central Region → Texas → Houston (Postal: 77095)
East Region → New York → New York City (Postal: 10035)
```

#### **Data Model Relationships**
```
Orders (Fact)
    ├── Customer ID → Customers.Customer ID (Many-to-One)
    ├── Product ID → Products.Product ID (Many-to-One)
    └── Postal Code → Location.Postal Code (Many-to-One)
```

#### **Data Quality Validation**
- ✅ Verified referential integrity: All foreign keys matched dimension records
- ✅ Confirmed no duplicate Customer IDs (795 unique customers)
- ✅ Validated date logic: Ship Date ≥ Order Date (no future orders)
- ✅ Business logic check: Profit = Sales - Costs (including legitimate negative profits from deep discounts)
- ✅ Statistical ranges: Sales $0.44 - $22,638 per line, Discounts 0% - 45%

---

## 🎯 Dashboard Architecture

### **Action: Dashboard Development**

I created **two interconnected dashboards** with consistent design language and cross-filtering capabilities:

### **1️⃣ Sales Dashboard** - Executive Performance Overview
**Purpose**: Analyze revenue trends, profit margins, and product performance with multi-year context

**Components**:
- **3 Primary KPI Cards**: Total Sales, Total Profit, Total Quantity (with YoY % change)
- **3 Trend Line Charts**: Monthly performance trends with highest/lowest month markers
- **Subcategory Comparison**: Horizontal bar chart comparing current vs. previous year by product line
- **Profit Analysis**: Color-coded profit bars identifying winners (blue) and loss-makers (orange)
- **Time Series Analysis**: Dual-axis chart showing sales & profit trends against average benchmarks

### **2️⃣ Customer Dashboard** - Client Behavior & Retention
**Purpose**: Track customer growth, order activity, and identify high-value clients

**Components**:
- **3 Primary KPI Cards**: Total Customers, Sales per Customer, Total Orders (with YoY % change)
- **Top 10 Customers Table**: Ranked by profit contribution with recency tracking
- **Customer Distribution Histogram**: Order frequency segmentation (1-8+ orders per customer)
- **Trend Line Charts**: Monthly customer and order trends with growth indicators

### **Interactive Filter Panel**
All dashboards share a unified filter panel:
- **Select Year** (Parameter): Dynamic year selection (2020-2023)
- **Product Hierarchy**: Category → Sub-Category filters
- **Geographic Hierarchy**: Region → State → City filters

**Filter Behavior**: All visualizations auto-update based on selected filters using parameter actions and cascading filter logic.

---

##  Advanced Calculations & LOD Expressions

### **Action: Technical Implementation**

I implemented **30+ calculated fields** using Level of Detail (LOD) expressions, table calculations, and conditional logic to enable sophisticated analysis:

### **Complete KPI Calculation Reference**

| **KPI Name** | **Calculation Type** | **LOD Used?** | **Visual Indicator** | **Purpose** |
|--------------|---------------------|---------------|---------------------|-------------|
| **Total Sales CY** | SUM([Sales]) | No | Number | Current year revenue |
| **Total Profit CY** | SUM([Profit]) | No | Number | Current year profit |
| **Total Quantity CY** | SUM([Quantity]) | No | Number | Current year units sold |
| **PY Sales** | FIXED LOD | ✅ Yes | Hidden (used in calc) | Previous year baseline |
| **PY Profit** | FIXED LOD | ✅ Yes | Hidden (used in calc) | Previous year baseline |
| **PY Quantity** | FIXED LOD | ✅ Yes | Hidden (used in calc) | Previous year baseline |
| **% Diff Sales vs PY** | ((CY-PY)/PY)*100 | Uses LOD | ✅ ▲▼ Green/Red Arrow | YoY growth indicator |
| **% Diff Profit vs PY** | ((CY-PY)/PY)*100 | Uses LOD | ✅ ▲▼ Green/Red Arrow | YoY profitability trend |
| **% Diff Quantity vs PY** | ((CY-PY)/PY)*100 | Uses LOD | ✅ ▲▼ Green/Red Arrow | YoY volume trend |
| **Highest Month Sales** | FIXED LOD (MAX) | ✅ Yes | 🔵 Blue Dot | Peak performance marker |
| **Lowest Month Sales** | FIXED LOD (MIN) | ✅ Yes | 🟠 Orange Dot | Trough performance marker |
| **Sales Difference MoM** | Table Calc (LOOKUP) | No | ✅ ▲▼ in Tooltip | Month-over-month change |
| **Avg Sales Benchmark** | WINDOW_AVG | No | Dotted Line | Performance threshold |
| **Avg Profit Benchmark** | WINDOW_AVG | No | Dotted Line | Profitability threshold |
| **Above/Below Avg** | IF > AVG | No | Blue/Orange Bars | Performance categorization |
| **Sales by Sub-Category CY** | SUM (filtered) | No | Dark Bars | Current year breakdown |
| **Sales by Sub-Category PY** | SUM (filtered) | No | Gray Bars | Prior year comparison |
| **Profit by Sub-Category** | SUM([Profit]) | No | Blue/Orange Bars | Category profitability |
| **Total Customers CY** | COUNTD([Customer ID]) | No | Number | Active customer base |
| **PY Customers** | FIXED LOD | ✅ Yes | Hidden | Previous year baseline |
| **% Diff Customers vs PY** | ((CY-PY)/PY)*100 | Uses LOD | ✅ ▲▼ Green/Red Arrow | Customer growth rate |
| **Sales Per Customer** | SUM/COUNTD | No | Number ($) | Average customer value |
| **PY Sales Per Customer** | FIXED LOD ratio | ✅ Yes | Hidden | Previous year baseline |
| **% Diff Sales/Customer** | ((CY-PY)/PY)*100 | Uses LOD | ✅ ▲▼ Green/Red Arrow | Customer value trend |
| **Total Orders CY** | COUNTD([Order ID]) | No | Number | Transaction volume |
| **PY Orders** | FIXED LOD | ✅ Yes | Hidden | Previous year baseline |
| **% Diff Orders vs PY** | ((CY-PY)/PY)*100 | Uses LOD | ✅ ▲▼ Green/Red Arrow | Order frequency trend |
| **Customer Profit Rank** | RANK_UNIQUE | No | Rank Number (1-10) | Top customer identification |
| **Profit Margin %** | (Profit/Sales)*100 | No | ✅ ▲ if >89% | Customer profitability |
| **Last Order Date** | FIXED LOD (MAX) | ✅ Yes | Date | Recency for RFM analysis |
| **Orders per Customer** | FIXED LOD (COUNTD) | ✅ Yes | Histogram Bins | Frequency segmentation |

---

## 💡 Key LOD Expressions Explained

### **1. Previous Year Metrics (FIXED LOD)**
```tableau
PY Sales = 
{FIXED : 
    SUM(IF YEAR([Order Date]) = [Selected Year Parameter] - 1 
    THEN [Sales] END)
}
```
**Why LOD?** Ensures Previous Year calculation ignores all dashboard filters (Region, Category, etc.) except the year parameter, preventing incorrect comparisons when users drill down into specific segments.

### **2. Highest/Lowest Month Detection (Nested LOD)**
```tableau
Highest Month = 
{FIXED YEAR([Order Date]) : 
    MAX(IF SUM([Sales]) = 
        {FIXED YEAR([Order Date]), MONTH([Order Date]) : MAX(SUM([Sales]))} 
    THEN [Order Date] END)
}
```
**Why Nested?** Inner LOD finds the maximum sales value across all months; outer LOD identifies which specific month achieved that maximum. Enables dynamic blue dot markers on trend lines.

### **3. Orders per Customer (FIXED LOD for Histogram)**
```tableau
Orders per Customer = 
{FIXED [Customer ID] : COUNTD([Order ID])}
```
**Why LOD?** Aggregates to customer level before creating histogram bins, showing distribution of order frequency (e.g., "200 customers placed 1 order, 200 placed 2 orders").

### **4. Arrow Indicator Logic (Conditional Formatting)**
```tableau
// Step 1: Calculate percentage
Sales % Diff = ((SUM([Sales]) - [PY Sales]) / [PY Sales]) * 100

// Step 2: Create arrow symbol
Arrow Symbol = 
IF [Sales % Diff] > 0 THEN "▲"  
ELSE IF [Sales % Diff] < 0 THEN "▼"  
ELSE "—" END

// Step 3: Apply color
Arrow Color = 
IF [Sales % Diff] > 0 THEN "#2DC937"  // Green
ELSE IF [Sales % Diff] < 0 THEN "#E15759"  // Red
ELSE "#B0B0B0" END  // Gray
```
**Implementation**: Used in KPI cards to instantly communicate trend direction - green ▲ for growth, red ▼ for decline.

---

## 📈 Results & Business Insights

### **Result: Key Findings from 2023 Analysis**

#### **Sales Performance (2023)**
- **Total Sales**: $164.4M 
  - **Growth**: ▲ 20.6% vs. 2022 ($136.3M) - Strong revenue acceleration
- **Total Profit**: $27.9M
  - **Growth**: ▲ 30.4% vs. 2022 ($21.4M) - Profitability outpacing revenue (excellent margin improvement)
- **Total Quantity**: 12K units
  - **Growth**: ▲ 26.8% vs. 2022 (9.5K units) - Volume growth driving revenue

**Key Insight**: Profit growing faster than sales (+30.4% vs +20.6%) indicates improved operational efficiency, better product mix, or pricing optimization.

#### **Product Category Analysis**
**High-Profit Winners** (Blue bars):
- **Phones**: ~$42K profit - Top performing subcategory
- **Chairs**: ~$38K profit - Strong furniture segment
- **Accessories**: ~$36K profit - High-margin technology add-ons

**Loss-Making Categories** (Orange bars):
- **Tables**: ~-$8K loss - Despite sales, unprofitable due to discounting
- **Envelopes**: ~-$2K loss - Low-margin commodity item with price pressure

**Actionable Insight**: Recommend pricing strategy review for Tables (45% discount example seen in data causing -$383 loss on single order) and potential discontinuation of Envelopes subcategory.

#### **Seasonal Trends**
- **Peak Month**: December (~$8,000K sales) - Holiday season surge 🔵
- **Trough Month**: February (~$2,500K sales) - Post-holiday lull 🟠
- **Observation**: Clear seasonal pattern with Q4 spike, requiring inventory planning and promotional calendar alignment

---

#### **Customer Insights (2023)**
- **Total Customers**: 693
  - **Growth**: ▲ 8.6% vs. 2022 (638 customers) - Modest customer acquisition
- **Total Orders**: 1,687
  - **Growth**: ▲ 28.3% vs. 2022 (1,315 orders) - Significantly outpacing customer growth
- **Sales Per Customer**: $1,058
  - **Growth**: ▲ 10.8% vs. 2022 ($955) - Increasing customer value

**Critical Insight**: Orders growing 3.3x faster than customers (28.3% vs 8.6%) indicates **existing customers are ordering more frequently** rather than relying on new customer acquisition - a positive retention signal.

#### **Customer Segmentation**
**Top 10 Customers by Profit** (Combined $22K+ profit):
1. **Raymond Buch** - $6,781 profit (97.1% margin) - Last order: 25-09-2023
2. **Hunter Lopez** - $5,046 profit (97.5% margin) - Last order: 17-11-2023
3. **Tom Ashbrook** - $4,599 profit (101.6% margin) - Last order: 22-10-2023

**Key Insight**: Top 10 customers show 89-101% profit margins, suggesting premium product purchases or corporate accounts. **Retention strategy critical** - losing even one could significantly impact profitability.

#### **Order Frequency Distribution**
- **~400 customers**: 1-2 orders (low engagement - **activation opportunity**)
- **~156 customers**: 3 orders (moderate engagement)
- **~100 customers**: 4-5 orders (core repeat buyers)
- **~39 customers**: 5+ orders (VIP segment - **loyalty program candidates**)

**Actionable Insight**: 
- **57% of customers** (400/693) placed only 1-2 orders - **implement onboarding campaigns** to drive second purchase
- **6% of customers** (39/693) placed 5+ orders - **create VIP loyalty program** with exclusive perks

---

## 🎯 Business Recommendations

Based on dashboard analysis, I recommend:

### **1. Pricing & Product Strategy**
- **Immediate**: Review discount policies for Tables subcategory (causing losses despite sales volume)
- **Short-term**: Consider discontinuing Envelopes or renegotiating supplier costs
- **Long-term**: Expand Phones, Chairs, Accessories inventory (high-profit categories)

### **2. Customer Retention & Growth**
- **High Priority**: Implement VIP retention program for Top 10 customers (worth $22K+ profit)
- **Activation Campaign**: Target 400 single-purchase customers with second-order incentives
- **Loyalty Program**: Create tiered rewards for customers with 5+ orders to increase frequency

### **3. Seasonal Planning**
- **Q4 Preparation**: Increase inventory 3x for November-December based on historical surge
- **Q1 Promotions**: Launch February campaigns to offset post-holiday slump

### **4. Data-Driven Culture**
- **Dashboard Adoption**: Train regional managers to use filters for their territory analysis
- **Monthly Reviews**: Establish KPI review cadence using these dashboards as single source of truth

---

## 🛠️ Technical Skills Demonstrated

### **Tableau Capabilities**
✅ **Level of Detail (LOD) Calculations** - FIXED, INCLUDE expressions for complex aggregations  
✅ **Parameter Actions** - Dynamic year selection driving all calculations  
✅ **Table Calculations** - WINDOW_AVG, LOOKUP for moving averages and MoM comparisons  
✅ **Conditional Formatting** - Arrow indicators, color-coded performance bars  
✅ **Dual-Axis Charts** - Trend lines with benchmark overlays  
✅ **Custom Tooltips** - Rich hover information with contextual comparisons  
✅ **Dashboard Actions** - Cross-filtering between visualizations  
✅ **Hierarchical Filtering** - Cascading filters from Region → State → City  

### **Data & Analytics Skills**
✅ **Dimensional Modeling** - Star schema design with fact/dimension tables  
✅ **Data Quality Validation** - Referential integrity, business logic checks  
✅ **KPI Engineering** - 30+ calculated metrics with business context  
✅ **Trend Analysis** - YoY comparisons, seasonality detection  
✅ **Customer Analytics** - RFM framework foundation, segmentation  
✅ **Business Storytelling** - Insights translated to actionable recommendations  

---

## 📊 Dashboard Preview

### Sales Dashboard (2023)
- **Top Section**: 3 KPI cards with trend lines and YoY arrows
- **Middle Section**: Subcategory comparison bars (current vs. previous year)
- **Bottom Section**: Profit analysis by subcategory + Time series dual-axis chart

### Customer Dashboard (2023)
- **Top Section**: 3 customer KPI cards with trend lines
- **Middle Left**: Customer distribution histogram (order frequency)
- **Middle Right**: Top 10 customers table with profit ranking
- **Filter Panel**: Year, Product, Location selectors

---

## 🔗 Repository Contents

```
📁 Sales-Customer-Analytics-Tableau/
├── 📊 Dashboards/
│   ├── Sales_Dashboard_2023.twbx (Tableau workbook)
│   └── Customer_Dashboard_2023.twbx (Tableau workbook)
├── 📁 Datasets/
│   ├── Orders.csv (9,996 rows - Fact table)
│   ├── Products.csv (1,896 rows - Dimension)
│   ├── Location.csv (634 rows - Dimension)
│   └── Customers.csv (795 rows - Dimension)
├── 📁 Documentation
│   ├── README.md (This file)
│   ├── Calculation_Reference.md (All 30 LOD expressions)
│   └── Data_Model_Diagram.png
```

---

## 🚀 Tools & Technologies

- **Tableau Desktop** (2023) - Dashboard development
- **Microsoft Excel** - Initial data validation
- **Dimensional Modeling** - Star schema design
- **LOD Calculations** - FIXED, INCLUDE expressions
- **Parameter Actions** - Dynamic filtering

---

## 📧 Contact & Portfolio

**Created by**: [Yash Gadhave]  
**Program**: MSc Data Science (Final Year)  

---

## 📝 License

This project is part of an academic portfolio for job applications and is available for educational purposes.


