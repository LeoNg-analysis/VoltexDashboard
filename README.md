# Voltex Sales Performance Dashboard

An end-to-end Power BI dashboard built on a fictional electronics retailer ("Voltex") sales dataset, covering growth, operations, product, and customer performance in a single, interactive report.

> 📌 Assumption: this README is written in English (GitHub convention). Let me know if you'd like a Vietnamese version instead.

---

## 📖 Overview

This project goes beyond a simple drag-and-drop report — the focus was on building a solid semantic model (fact/dimension tables, 60+ DAX measures, dynamic Month-over-Month indicators, and a Field Parameter-driven metric switcher) so the dashboard can actually answer business questions, not just visualize data.

**File:** `Voltex_Sales_Performance_Dashboard.pbix`

---

## 🗂️ Pages

| Page | Focus |
|---|---|
| **Growth** | Overall sales growth trends |
| **Operation** | Fulfillment performance — OTD Rate, Delivery Days, Processing Hours, shipping partner comparison, return cost by state |
| **Product** | Product-level performance — revenue/profit by category, top products, value tier mix, return reasons |
| **Customer** | Customer segmentation — active customers, retention, satisfaction, AOV/LTV, demographics, fulfillment status |

---

## 🧱 Data Model

| Table | Role | Key columns |
|---|---|---|
| `fact_order` | Fact table | Order_ID, Customer_ID, Product_ID, Order_Date, Order_Quantity, Order_Discount, Is_Returned, Feedback_Rating, Feedback_Sentiment, delivery_status |
| `dim_customer` | Dimension | Customer_ID, Customer_Age, Customer_Gender, Age_Group |
| `dim_product` | Dimension | Product_ID, Product_Category, Product_Brand, Product_ValueTier, Product_UnitPrice, Product_UnitCOGS |
| `dim_region` | Dimension | Region_Name, Region_State, Region_DistributionCenter |
| `dim_return` | Dimension | Return_ID, Return_Reason |
| `dim_calendar` | Date table | Date, Year, Month, MonthName |

Star schema, all dimension tables connected to `fact_order` via single-direction relationships.

---

## 🧮 Key DAX Measures

Measures are organized into display folders for easy navigation in the Fields pane:

```
01_Orders          — Total Orders (+ MoM)
02_Revenue         — Total Revenue (+ MoM)
03_Cost            — Total Cost, Total COGS
04_Profit          — Total Profit, Profit Margin
05_Return          — Return Rate, Return Cost, Returned Orders Revenue, % Revenue Lost to Returns
06_Rating          — Avg Rating (+ MoM)
07-09_*            — OTD Rate, Cycle Time, OpEx per Order
10_Customer        — Active Customers, Retention Rate, AOV, LTV, Customer segmentation %
11_Product         — Active Products, Units Sold, Avg Selling Price
```

Notable techniques used:
- **Month-over-Month pattern**: every core KPI has a matching `_MoM_Perc`, `_MoM_Icon`, `_MoM_Color`, `_MoM_Label` measure, built on shared helper functions (`MoM_Perc`, `MoM_Icon`, `Delta_Color`) for consistent styling across the whole report.
- **Field Parameters**: the Product page's "Key Indicators" selector lets users switch the Y-axis metric of a chart (Revenue / Profit Margin / Units Sold / Return Rate / Avg Selling Price) without needing a separate chart per metric.
- **New customer detection**: identifies a customer's true first-ever order date (ignoring the active date filter) to correctly flag new vs. returning customers per period.

---

## 💡 Key Insights Surfaced by the Model

- **Revenue leakage from returns is invisible by default** — the standard `Total Revenue` measure excludes returned orders entirely. Once isolated, returned orders account for **~8.6% of potential revenue**.
- **Return cost is far more concentrated than return count**: "Defective" products make up 57% of returned orders but **77.8% of total return cost** — a strong signal to prioritize product quality over other return causes (changed mind, wrong item, etc.).
- **Customer Lifetime Value > Average Order Value** by a wide margin, reinforcing that customer-level metrics tell a different (and often more important) story than order-level averages.

---

## 🛠️ Tech Stack

- Power BI Desktop
- DAX (calculated columns, measures, Field Parameters)
- Star schema data modeling

---

## 🚀 Getting Started

1. Clone this repo / download the `VoltexBusinessDashboard.pbix` file
2. Open with **Power BI Desktop** (recent version recommended for Field Parameter support)
3. If connected to a live data source, update credentials under **File → Options and settings → Data source settings**
4. Refresh the model if needed
