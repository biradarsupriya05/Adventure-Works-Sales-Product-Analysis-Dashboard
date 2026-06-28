# Adventure-Works-Sales-Product-Analysis-Dashboard

### 🏢 Company Overview

Adventure Works Cycles is a multinational bicycle manufacturing company headquartered in Bothell, Washington. It produces and sells metal and composite bicycles across North America, Europe, and Asia. The company also operates a manufacturing plant in Mexico for key bicycle components and focuses on expanding market share, increasing sales to high-value customers, growing online sales, and reducing production costs.

### 📌 Project Overview

The Adventure Works Sales & Product Analysis project uses SQL to analyze business data and uncover insights into sales, profit, products, customers, and regional performance. 

By writing SQL queries, the project identifies key trends, top-performing products, customer behavior, and sales patterns to support data-driven business decisions.

### 📌 KPI Cards

## 📌 Key Performance Indicators (KPIs)

| KPI | Value |
|------|------:|
| 💰 Total Sales | **29M** |
| 📈 Total Profit | **12M** |
| 💵 Total Cost | **17M** |
| 👥 Total Customers | **18K** |
| 📦 Total Quantity Sold | **60K** |
| 🎯 Profit Margin | **41%** |

### Sales Analysis

### Monthly Sales Trends
<img width="705" height="318" alt="Monthly Sales Trend" src="https://github.com/user-attachments/assets/6c8e3233-03f2-4548-bf4e-24b207e0743b" />

```sql
SELECT
    MonthFullName,
    CONCAT(ROUND(SUM(SalesAmount) / 1000000, 2), ' M') AS TotalSales
FROM fact_sales
GROUP BY MonthFullName
ORDER BY MonthFullName;
```

- Sales showed an upward trend throughout the year.

- December recorded the highest monthly sales (3.21M), while February had the lowest (1.74M).

### Business Insight: 

Peak sales during December indicate seasonal buying behavior, making Q4 a critical period for maximizing revenue.

  ###  Yearly Trend

  <img width="478" height="323" alt="Yearly Sales Trend" src="https://github.com/user-attachments/assets/55fcb218-b429-4073-bf82-2a6eb37326b1" />

```sql
  SELECT 
    Year,
    CONCAT(ROUND(SUM(SalesAmount)/1000000, 2), ' M') AS TotalSales
FROM fact_sales
GROUP BY Year
ORDER BY SUM(SalesAmount) DESC;
```

- 2013 was the best-performing year with 16.35M in sales.

- Sales were comparatively lower in 2010 and 2014.

### Business Insight: 

Analyzing the factors behind 2013's success can help replicate growth strategies in future years.

### Quarterly Trend

<img width="373" height="478" alt="Quartely Trend" src="https://github.com/user-attachments/assets/ee9166f1-f939-4967-8725-df082df2568e" />

```sql
SELECT 
    Quarter,
    CONCAT(ROUND(SUM(SalesAmount)/1000000, 2), ' M') AS TotalSales
FROM fact_sales
GROUP BY Quarter
ORDER BY SUM(SalesAmount) DESC;
```
- Q4 recorded the highest sales (9.11M), while Q1 recorded the lowest (5.52M).

### Business Insight: 

Strong year-end demand suggests opportunities for seasonal promotions and inventory optimization.

### Yearwise Sales And Production Amount

<img width="433" height="484" alt="Sales Vs Cost Trend" src="https://github.com/user-attachments/assets/fe4df9da-0e80-4a2d-b946-9e78595cfa3a" />

- Sales consistently remained above product costs, indicating strong profitability.

- 2013 recorded the highest sales and business performance.

### Business Insight: 

Revenue growth outpaced costs, demonstrating efficient operational and pricing strategies.

### Top 5 Customer by Sales

<img width="445" height="481" alt="Top 5 Customer by Sales" src="https://github.com/user-attachments/assets/6ffa475f-bf91-4b28-985d-a682a37dab07" />

```sql
SELECT
    CONCAT(c.FirstName, ' ', c.LastName) AS Customer_Name,
    CONCAT(ROUND(SUM(SalesAmount) / 1000,1), 'K') AS Total_Sales
FROM Fact_Sales f
JOIN dimCustomer c
    ON f.CustomerKey = c.CustomerKey
GROUP BY
    c.FirstName,
    c.LastName
ORDER BY
    SUM(SalesAmount) DESC limit 5;
```

- ordan Turner is the top customer with around 16K in sales.

- The remaining top customers each contributed approximately 13K, showing a balanced customer base.

### Business Insight: 

Retaining high-value customers through loyalty programs can drive long-term revenue.

### Top 5 Sales by Region

<img width="521" height="318" alt="Top 5 Sales By Region" src="https://github.com/user-attachments/assets/2b0a0293-d326-4c67-832a-e3a4a164c278" />

```sql
SELECT
    S.SalesTerritoryRegion AS Region,
    CONCAT(ROUND(SUM(f.SalesAmount) / 1000000.0, 2), 'M') AS Total_Sales
FROM Fact_Sales f
JOIN dimsalesterritory S
    ON f.SalesTerritoryKey = S.SalesTerritoryKey
GROUP BY
    S.SalesTerritoryRegion
ORDER BY
    SUM(f.SalesAmount) DESC limit 5;
```
- Australia generated the highest sales (9.06M).

- Southwest and Northwest were the next strongest-performing regions.

### Business Insight: 

High-performing regions should remain key investment areas, while lower-performing regions present expansion opportunities.

### Product Analysis

<img width="1916" height="982" alt="Product Analysis Tableau" src="https://github.com/user-attachments/assets/3b1c1cfc-6479-4f43-aa86-ec1d23951767" />


```sql
select p.EnglishProductName,
concat(Round(sum(f.salesAmount)/1000000,2),'M') as Total_Sales
from fact_sales f join dimproduct  p
on f.productkey=p.productkey
group by p.EnglishProductName
order by sum(f.salesAmount)  desc limit 10;
```

- Mountain-200 bike models dominated product sales.

- The highest-selling product generated approximately 1.37M in revenue.

### Business Insight: 

Expanding the top-selling product line can further increase revenue and market share.

### Top 5 Products by Profit

<img width="627" height="379" alt="Top 5 Product by Profit" src="https://github.com/user-attachments/assets/eca4f8a5-d655-4dd4-bdf6-88fa878ce897" />

```sql
select p.EnglishProductName,
concat(Round(sum(f.profit)/1000),' K') as Profit
from fact_sales f join dimproduct  p
on f.productkey=p.productkey
group by p.EnglishProductName
order by sum(f.profit)  desc limit 5;
```
- Mountain-200 products also generated the highest profit.

- The top product earned approximately 626.6K in profit.

### Business Insight: 

Focusing on high-margin products can significantly improve overall profitability.

### Sales by Subcategory

<img width="624" height="437" alt="Top 5 Subcategory by Sales" src="https://github.com/user-attachments/assets/50d28fc0-0059-4014-b807-b523bfce48e9" />

```sql
select s.EnglishProductSubcategoryName,
concat(Round(Sum(f.SalesAmount)/1000000, 2), ' M') as Total_Sales
from fact_sales as f join dimproduct as p
on f.productkey=p.productkey join
dimproductsubcategory as s 
on p.ProductSubcategoryKey=s.ProductSubcategoryKey
group by s.EnglishProductSubcategoryName
order by Sum(f.SalesAmount) desc limit 5;
```

- Road Bikes generated the highest sales (14.52M).

- Mountain Bikes and Touring Bikes were the next top-performing subcategories.

### Business Insight: 
Increasing investment in top-performing subcategories can accelerate revenue growth

### Top 5 Product by category

<img width="539" height="439" alt="Sales by Category" src="https://github.com/user-attachments/assets/46a0241d-f401-4bd0-8425-aa97adef19fe" />

```sql
select c.EnglishProductCategoryName,
concat(Round(Sum(f.SalesAmount)/1000000, 2), ' M') as Total_Sales
from fact_sales as f join dimproduct as p
on f.productkey=p.productkey join
dimproductsubcategory as s 
on p.ProductSubcategoryKey=s.ProductSubcategoryKey
join dimproductcategory as c 
on s.ProductCategoryKey=c.ProductCategoryKey
group by c.EnglishProductCategoryName
order by Sum(f.SalesAmount) desc limit 5;
```

- Bikes contributed the majority of total sales (28.32M).

- Accessories and Clothing contributed a relatively small share of overall revenue.

### Business Insight: 

Cross-selling accessories and clothing with bike purchases can increase average order value and overall revenue.

### Top 5 sales by color

<img width="533" height="383" alt="Top 5 Sales by Color" src="https://github.com/user-attachments/assets/89e7474d-111a-4eae-93b2-717c0e52b419" />

```sql
select p.color,
concat(Round(Sum(f.SalesAmount)/1000000, 2), ' M') as Total_Sales
from fact_sales as f join dimproduct as p 
on f.productkey=p.productkey
group by p.color
order by Sum(f.SalesAmount) desc limit 5;
```
- Black was the most preferred product color.

- Red and Silver were the next best-selling colors.
  
- Business Insight: Customer color preferences should guide inventory planning and future product offerings.

### 💡 Overall Business Insights

* Generated 29M in total sales with 12M in profit, achieving a strong 41% profit margin, indicating a healthy and profitable business.
* Bikes are the primary revenue driver, contributing 28.32M of total sales, while Road Bikes are the best-performing subcategory.
* Mountain-200 bike models consistently rank as the top-selling and most profitable products.
* Sales peak during Q4, with December recording the highest monthly sales, highlighting strong seasonal demand.
* Australia is the highest-performing sales region, presenting opportunities for continued investment and market expansion.
* A relatively balanced contribution from top customers reduces dependence on a single customer while supporting stable revenue growth.
* Lower-performing categories such as Accessories and Clothing offer opportunities for cross-selling and revenue diversification.
* The analysis provides valuable insights for optimizing product strategy, inventory planning, regional sales, and customer engagement to drive future business growth.
