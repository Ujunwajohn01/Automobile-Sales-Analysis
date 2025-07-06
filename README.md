# Automobile-Sales-Analysis
An SQL-Based Data Analytics Project of retail automobile sales that uses advanced SQL queries to uncover key business metrics, sales performance. 

### Table of Contents
- Project Overview
- Project Scope
- Business Objectives
- Data Source
- Dataset Overview
- Data Cleaning and Processing
- Key KPIs
- Key Analysis & Business Insights
- Strategic Recommendations
- Conclusion
- Contact

### Project Overview
This project analyzes historical sales transactions from a wholesale distributor of collectible and luxury model vehicles, including Classic Cars, Motorcycles, Planes, Trains, Ships, and other premium product lines. The dataset simulates a real-world retail ERP system and captures orders placed by various customers across the globe between 2003 and 2005.
The goal of this analysis is to uncover performance trends across products, customer segments, regions, and time — to help inform strategic decisions in sales, marketing, and operations. Using SQL, the project evaluates revenue drivers, top markets, seasonal sales patterns, and customer behavior through a series of business questions and KPIs.

### Business Objectives
The main objectives of this comprehensive analysis is to:
- Quantify overall business performance using sales, orders, and revenue.
- Identify high-performing product lines and deal sizes.
- Analyze customer distribution by geography and product engagement.
- Discover temporal patterns in revenue (monthly and yearly).
- Measure growth through MoM and YoY sales trends.
- Recommend strategies to boost sales and customer satisfaction.

### Expected Outcomes
- A clear view of top-performing products, customers, and locations.
- Temporal insights that reveal peak and slow sales periods.
- Segmentation of deal sizes by sales value.
- Identification of customer concentration by country and product category.
- Calculation of sales growth patterns with actionable recommendations.

### Use Case
This report can be used by sales managers and executives of the company to monitor key sales drivers, evaluate operational performance, and make informed decisions around product inventory, promotions, and regional focus. It is also beneficial for business analysts working on retail intelligence or CRM systems looking to replicate SQL-based analytics frameworks.


### Data Source
- This dataset was gotten from [Kaggle.com](https://www.kaggle.com/datasets/kyanyoga/sample-sales-data?resource=download), a data science community with powerful tools and resources to support advancements in data science. 
- Source Table: `retail_sales_data_`
- Key Columns: ORDERNUMBER, QUANTITYORDERED, SALES, PRODUCTLINE, COUNTRY, CITY, STATUS, YEAR_ID, MONTH_ID, ORDERDATE, CUSTOMERNAME, CONTACTFIRSTNAME, CONTACTLASTNAME, DEALSIZE
- Created Columns: MONTHNAME, CONTACTFULLNAME
- Time Period: 2003–2005
- Dataset Type: `Transaction-level historical sales log` meanning that each row represents a single past sales transaction — including what was sold, to whom, when, and for how much.
It’s detailed, time-based data collected over a period (2003–2005), not summaries or aggregates.
This format allows deep analysis of trends, customer behavior, product performance, and sales growth.

### Dataset Overview
The dataset used in this project is a transaction-level historical sales log from a global distributor of collectible and luxury model vehicles, including product lines such as Classic Cars, Motorcycles, Planes, Trains, Ships, and Vintage Cars. It captures detailed order data across multiple countries between 2003 and 2005, providing rich context for sales performance, customer behavior, and business growth analysis.

Each row in the dataset represents a single sales transaction, including information about the order number, date, product line, quantity ordered, sales revenue, deal size, shipping status, customer details, and geographic location. This structure enables both granular transaction analysis and high-level trend insights.

- **Data Dictionary**

| Column Name           | Description                                                  |
|-----------------------|--------------------------------------------------------------|
| `ORDERNUMBER`         | Unique identifier for each order                             |
| `ORDERDATE`           | Date the order was placed                                    |
| `PRODUCTLINE`         | Category of the product sold (e.g., Classic Cars)            |
| `QUANTITYORDERED`     | Number of items ordered                                      |
| `SALES`               | Revenue generated from the order                             |
| `DEALSIZE`            | Size of the deal (Small, Medium, Large)                      |
| `STATUS`              | Order status (e.g., Shipped, Cancelled)                      |
| `CUSTOMERNAME`        | Name of the customer/company                                 |
| `COUNTRY`             | Country where the customer is located                        |
| `CITY`                | City where the customer is located                           |
| `YEAR_ID`             | Year the order was placed                                    |
| `MONTH_ID`            | Month (as a number) the order was placed                     |
| `CONTACTFIRSTNAME`    | First name of the customer contact                           |
| `CONTACTLASTNAME`     | Last name of the customer contact                            |


- **Usefulness**
  
This dataset is ideal for performing:

- Sales performance analysis (KPIs like revenue, orders, quantity sold)
- Customer segmentation
- Trend analysis (MoM and YoY growth)
- Geographic insights
- Product category performance
- Operational insights

### Data Cleaning and Processing 
Before performing any analysis, the dataset underwent a structured cleaning and preprocessing phase to ensure reliability, consistency, and analytical usability. The following steps were taken using SQL:

**1. Date Standardization:**
- Created a new column called Month Name to enable time-series and seasonality analysis (e.g., monthly revenue trends):
```sql
ALTER TABLE
            retail_sales_data_
ADD
            [Month Name] VARCHAR(20)
```

- Extracted the month name from the ORDERDATE column using SQL’s DATENAME(Month, ORDERDATE) function:
```sql
Update
       retail_sales_data_
Set
       [Month Name] = DATENAME(Month,orderdate)
```

- Verified date formats to ensure ORDERDATE was consistently in a valid datetime format.

**2. Creating a Full Contact Name**
- Concatenated CONTACTFIRSTNAME and CONTACTLASTNAME to form a new field CONTACTFULLNAME. This allowed for better customer identification and simplified reporting in visual dashboards or customer-level insights.
```sql
Alter Table
            retail_sales_data_
Add
            [CONTACTFULLNAME] Varchar(50)
```
```sql
Update
       retail_sales_data_
Set
       CONTACTFULLNAME = [CONTACTFIRSTNAME] + ' ' + [CONTACTLASTNAME]
```
**3. Ensuring Column Consistency**
- Reviewed all key columns (SALES, QUANTITYORDERED, STATUS, DEALSIZE, PRODUCTLINE, COUNTRY) for missing or inconsistent values.
- Ensured no critical NULL values were present in fields used for aggregation or grouping.
- Trimmed potential leading/trailing spaces in categorical fields (e.g., Productline, Status) to avoid grouping errors.

**4. Data Type Checks**
- Confirmed that numeric fields like SALES, QUANTITYORDERED, and MONTH_ID were in proper numerical format (FLOAT/INT) to allow mathematical computations.
- Ensured YEAR_ID was stored as a 4-digit integer to support Year-over-Year calculations.
- Validated that DEALSIZE was treated as a categorical variable and not misread as a text string.

### Key KPIs

| KPI                             | Value                | Insight |
|---------------------------------|----------------------|---------|
| **Total Revenue Generated**     | $10,032,628.85       | Total sales over 3 years — reflects overall business scale. |
| **Total Orders Placed**         | 2,823 orders         | Total transactions completed — useful for calculating averages. |
| **Total Quantity Ordered**      | 99,067 units         | Indicates total product movement — supports inventory planning. |
| **Average Order Value (AOV)**   | $3,554.40            | Shows average customer spend per order (Revenue ÷ Orders). |
| **Top Product Line by Revenue** | Planes – $502,671.80 | Indicates highest-performing category — strong sales driver. |
| **Top Country by Revenue**      | USA – $1,685,470.69  | Most profitable market — useful for regional strategy. |
| **Top Customer by Revenue**     | Euro Shipping Line – $912,294.11 | High-value account — vital for retention and relationship building. |

### Key Analysis & Business Insights
**1. Which product line generates the most revenue, and why does it matter?**

- **Product Line Performance Overview**

| Product Line     | Total Revenue | Total Orders  | Total Qty Ordered|  Total Customers |
|------------------|---------------|---------------|------------------|------------------|
| Classic Cars     | $3,919,615.66 | 967           | 33,992           | 89               |
| Motorcycles      | $1,166,388.34 | 331           | 11,663           | 49               |
| Planes           | $975,003.57   | 306           | 10,727           | 47               |
| Ships            | $714,437.13   | 234           | 8,127            | 48               |
| Trains           | $226,243.47   | 77            | 2,712            | 34               |
| Vintage Cars     | $1,903,150.84 | 607           | 21,069           | 84               |
| Trucks and Buses | 

**Why this matters:**
Understanding which product categories perform best helps the business focus its resources on the most profitable lines. It also reveals shifts in customer preferences and potential areas of saturation or growth.

**What the data shows:**
"Classic Cars" consistently lead in revenue, quantity ordered, and order volume — solidifying their role as the company’s flagship product. "Motorcycles" and "Planes" show steady growth year-over-year, indicating rising customer interest in those categories. Other lines, such as "Ships" and "Vintage Cars," remain flat or underperform.

**Business implications:**
- Classic Cars should remain central to inventory, marketing, and promotional strategy.
- Motorcycles and Planes represent promising growth areas — consider expanding variants, bundles, or promotions around these lines.
- Underperforming categories should be evaluated for potential retirement, redesign, or repositioning.

**Conclusion:**
Focusing on top-performing product lines while nurturing emerging ones will optimize revenue and reduce reliance on a single category.

- **Total Sales Per Product Line in each year (2003–2005)**
```sql
Select
      Productline, Year_Id,
Round
     (Sum (Sales), 2) as Total_Sales
From
     retail_sales_data_
Group by
     Productline, Year_Id
```  
```sql
---Pivoting the table above-----
Select
      Country, [2003], [2004], [2005]
From
      (Select Country, Year_Id,
      Round (Sum (Sales), 2) as Total_Sales
      From retail_sales_data_
      Group by Country, Year_Id) as DD
Pivot
      (Sum(Total_Sales)
      For Year_Id
      In ([2003], [2004], [2005])) as DD
```

| Product Line       | 2003          | 2004          | 2005         |
|--------------------|---------------|---------------|--------------|
| Classic Cars       | $1,484,785.29 | $1,762,257.09 | $672,573.28  |
| Motorcycles        | $370,895.58   | $560,545.23   | $234,947.53  |
| Planes             | $272,257.60   | $502,671.80   | $200,074.17  |
| Ships              | $244,821.09   | $341,437.97   | $128,178.07  |
| Trains             | $72,802.29    | $116,523.85   | $36,917.33   |
| Trucks and Buses   | $420,429.93   | $529,302.89   | $178,057.02  |
| Vintage Cars       | $650,987.76   | $911,423.77   | $340,739.31  |


**Interpretation & Insights:**
- Classic Cars dominate in all three years, with 2004 as the peak year, accounting for over $1.76 million in sales.
- Vintage Cars are a strong second, maintaining high performance across all years.
- Planes and Motorcycles saw significant growth between 2003 and 2004, before slightly declining in 2005 — likely due to shifting demand or operational limits.
- Trains and Ships are lower-revenue categories, with relatively flat performance.
- Trucks and Buses, though niche, consistently performed well — over $1.1 million combined across 3 years.

**Yearly Trends:**
- 2004 was the strongest year across almost all product lines — indicating a possible economic upswing, successful campaigns, or optimized operations. Several lines declined in 2005 — most notably Classic Cars — suggesting either market saturation or resource shift.

- **Percentage Contribution to Total Sales**

```sql
-----Total Sales by Each ProductLine------
Select
      Productline,
Round
      (Sum(sales),2) As Total_revenue
From
     retail_sales_data_
Group by
     Productline
```  
```sql
-----% Contributed = (total sales per cstegory/overall total sales)*100-----
Select
      Productline,
Round
      (Sum(Sales), 2) AS Total_revenue,
CONCAT
      (Round(Sum(SALES) * 100.0 / Sum(Sum(SALES)) OVER (), 2), '%') AS PercentageContribution
From
      retail_sales_data_
Group by
      Productline
Order by
      PercentageContribution DESC
```  

| Product Line     | Contribution % |
|------------------|----------------|
| Classic Cars     | 39.07%         |
| Vintage Cars     | 18.97%         |
| Motorcycles      | 11.63%         |
| Planes           | 9.72%          |
| Ships            | 7.12%          |
| Trains           | 2.26%          |
| Trucks and Buses | 11.24%         |

**Insight:** 
Classic Cars dominate the portfolio, contributing over 39% of total revenue, followed by Vintage Cars at 19%. While this highlights strong demand for collectible cars, it also reveals heavy dependence on a single category. Mid-tier lines like Motorcycles, Trucks, and Planes (each contributing around 10–11%) present valuable opportunities for diversification and growth. Lower-performing lines like Ships and Trains contribute less than 10% combined, and may require repositioning or niche targeting. Balancing product performance is essential for reducing risk and ensuring long-term revenue stability.

**2. Which deal sizes drive the most revenue — and what does this reveal about the customer base?**

**Why this matters:**
Segmenting revenue by deal size reveals how different types of customers contribute to overall performance. It helps shape pricing strategies, sales prioritization, and resource allocation.

**What the data shows:**
Medium deal sizes are the clear revenue leaders, contributing over 60% of total sales — making them the business’s backbone. Small deals come next, contributing around 26%, while Large deals surprisingly contribute the least, at just 13%.

**Business implications:**

- Focus efforts on nurturing the mid-market customer base, which delivers high volume and steady revenue.
- Use small deals to attract new customers and develop loyalty programs that encourage them to scale up.
- Reassess how large deals are approached — do they need better targeting, customized service, or better value propositions?

**Conclusion:**
Medium-sized transactions are your revenue engine. Understanding why large deals underperform could unlock untapped growth.

```sql
Select
      Dealsize,
Round
      (Sum(sales),2) As Total_revenue
From
      retail_sales_data_
Group by
      Dealsize
Order by
      Total_revenue DESC
```

| Deal Size | Total Revenue   |
|-----------|-----------------|
| Medium    | $6,087,432.24   |
| Small     | $2,643,077.35   |
| Large     | $1,302,119.26   |

3. Who are our most valuable customers, and how dependent are we on them?
Why this matters:
Identifying high-value customers helps prioritize retention efforts. Overreliance on a small group can pose a risk if even one major client is lost.

What the data shows:
The top customer — Euro Shipping Line — contributed over $900,000 in sales. The top 20 customers together represent a significant percentage of total revenue. This is a classic 80/20 scenario, where a small portion of customers drive a large share of business.

Business implications:

These clients are not just customers — they are strategic assets.

Assign dedicated account managers, offer premium perks, or early-access inventory to ensure retention.

Monitor their purchase behavior for early signs of churn or changing needs.

Conclusion:
Retaining top customers is more profitable than acquiring new ones. Treat them like partners — not transactions.

