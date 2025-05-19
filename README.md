# 📊 Power BI Major Project – README

![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20193537.png)

## 1. 🎯 Project Objective
To design an interactive and insightful Power BI dashboard that helps stakeholders analyze:

- Order trends  
- Delivery performance  
- Sales performance  
- Customer behavior  
- Reviews  
- Payment patterns  

The goal is to support data-driven decision-making using clean, well-modeled data.

---

## 2. 🔧 Flow of Work & Data Preparation

### 2.1 Initial Raw Data
- Input: 7 CSV files
- Tables:
  - `orders_dataset`
  - `customers_dataset`
  - `delivery_status_dataset`
  - `payment_mode_dataset`
  - `products_dataset`
  - `reviews_dataset`
  - `sellers_dataset`

 ![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20193850.png)

### 2.2 Power Query Data Cleaning
- Ensured correct data types
- Handled missing/null values (especially in date columns)
- Used Column Distribution to identify:
  - **Fact Table**: `orders_dataset`
  - **Dimension Tables**: all others listed above
 
![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20052759.png)
![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20052813.png)

### 2.3 Modeling Approach
- Built **1-to-many** relationships
- Designed a **Star Schema**
- Created a **Calendar table** with DAX

![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20051820.png)
![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20050025.png)

---

## 3. 🧩 Data Modeling

### 3.1 Schema Used
- **Star Schema** with `orders_dataset` as the central fact table

### 3.2 Table Categorization
- **Fact Table**: `orders_dataset`
- **Dimension Tables**:
  - `customers_dataset`
  - `delivery_status_dataset`
  - `payment_mode_dataset`
  - `products_dataset`
  - `reviews_dataset`
  - `sellers_dataset`

### 3.3 Relationships
| From Table (Column)           | To Table (Column)                   | Relationship |
|------------------------------|-------------------------------------|--------------|
| orders_dataset[customer_id]  | customers_dataset[customer_id]      | 1 → *        |
| orders_dataset[order_date]   | Calendar[Date] (Active)             | 1 → *        |
| orders_dataset[ship_date]    | Calendar[Date] (Inactive)           | 1 → *        |
| orders_dataset[order_id]     | delivery_status_dataset[order_id]   | 1 → *        |
| orders_dataset[order_id]     | payment_mode_dataset[order_id]      | 1 → *        |
| orders_dataset[order_id]     | reviews_dataset[review_id]          | 1 → *        |
| orders_dataset[product_id]   | products_dataset[product_id]        | 1 → *        |
| orders_dataset[seller_id]    | sellers_dataset[seller_id]          | 1 → *        |

### 3.4 Calendar Table
```DAX
Calendar = CALENDAR(MIN(orders_dataset[order_date]), MAX(orders_dataset[order_date]))
````

Additional columns:

```DAX
Order Year = YEAR(orders_dataset[order_date])
Order Month = FORMAT(orders_dataset[order_date], "MMMM")
```

### 3.5 Grouping

* Grouped `orders_dataset[order_date]` by:

  * Year
  * Month

---

## 4. ⚙️ Issues & Troubleshooting

### ❌ Problem

* Calendar slicer showed blank values.
* Selecting “blank” aggregated totals across all years.

### ✅ Solution

* Identified null values in `order_date` and `ship_date`
* Resolved by:

  * Filtering nulls in visuals
  * Updating DAX to ignore blank dates

---

## 5. 📐 DAX Measures Created

| Measure Name                  | Formula                                                                                                  |
| ----------------------------- | -------------------------------------------------------------------------------------------------------- |
| Total Orders                  | `DISTINCTCOUNT(orders_dataset[order_id])`                                                                |
| Revenue                       | `SUMX(orders_dataset, orders_dataset[price] * orders_dataset[quantity])`                                 |
| Average Order Value (AOV)     | `SUMX(orders_dataset, [revenue] / DISTINCTCOUNT(orders_dataset[order_id]))`                              |
| Shipped Quantity by Ship Date | `CALCULATE(SUM(orders_dataset[quantity]), USERELATIONSHIP('Calendar'[Date], orders_dataset[ship_date]))` |

---

## 6. ❓ Questions Raised and Solved

| **Question**                                        | **Status** |
| --------------------------------------------------- | ---------- |
| Year-to-date orders based on `order_date`?          | ✅ Solved   |
| What are the monthly revenue trends?                | ✅ Solved   |
| Who are the top-selling products and categories?    | ✅ Solved   |
| What are the total revenue and AOV?                 | ✅ Solved   |
| What is the distribution of payment methods?        | ✅ Solved   |
| Which regions contribute most to sales performance? | ✅ Solved   |

📸 **Screenshot Preview**:


![Sales and Revenue Dashboard](https://github.com/shubhamvermapersonal/powerbi_sales-revenueperformance/blob/591c99529c60e624404d5ac25fb3c74f011dd6a1/Screenshot%202025-05-19%20193505.png)

---

## 7. 💡 Additional Questions to Explore

### Sales & Trends

* Monthly revenue trends?
* Top-selling products and categories?
* Top revenue-generating sellers?

### Delivery Insights

* Average delivery time?
* % of late vs. on-time deliveries?
* Regional delivery efficiency?

### Payment Insights

* Most preferred payment methods?
* Trends in installment usage?

### Review Analysis

* Review score trends over time?
* Link between high scores and high prices?

### Regional & Time Trends

* Sales by region/state?
* Holiday vs. regular day sales?
* Weekend vs. weekday volume?

---

## 8. 📊 Dashboard Design Recommendations

### Suggested Visuals

| Visual            | Purpose                                       |
| ----------------- | --------------------------------------------- |
| Card KPIs         | Total Orders, Revenue, AOV, Avg Delivery Time |
| Line Chart        | Revenue trend over time                       |
| Bar Chart         | Top 10 Products by Revenue                    |
| Stacked Bar Chart | Sales by Category & Month                     |
| Donut Chart       | Review score distribution, Payment types      |
| Map Visual        | Customer orders by region                     |
| Matrix Table      | Product vs. Revenue and Order Volume          |
| Slicers           | Date, State, Category, Seller, Payment Type   |

### Interactivity Features

* Drill-down: year → month → day
* Tooltips on hover
* Dynamic filter panels
* Cross-highlight between visuals

---

## 9. ✅ Conclusion

* Built a fully functional, interactive Power BI dashboard using 7 datasets
* Applied star schema modeling for performance
* Created DAX measures to support advanced analytics
* Solved initial project questions and enabled deeper insights
* Ready for presentation and stakeholder review
