# Automobile-Sales-Analysis
An SQL-Based Data Analytics Project of retail automobile sales that uses advanced SQL queries to uncover key business metrics, sales performance. 

### Table of Contents
- [Project Overview](#project-overview)
- [Business Objectives](#business-objectives)
- [Expected Outcomes](#expected-outcomes)
- [Use Case](#use-case)
- [Data Source](#data-source)
- [Dataset Overview](#dataset-overview)
- [Data Cleaning and Processing](#data-cleaning-and-processing)
- [Key KPIs](#key-kpis)
- [Key Analysis & Business Insights](#key-analysis--business-insights)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)
- [Contact](#contact)


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

**1. Data Dictionary**

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


**2. Usefulness:**
  
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
| **Top Product Line by Revenue** | Classic Cars – $3,919,615.66 | Indicates highest-performing category — strong sales driver. |
| **Top Country by Revenue**      | USA – $1,685,470.69  | Most profitable market — useful for regional strategy. |
| **Top Customer by Revenue**     | Euro Shipping Line – $912,294.11 | High-value account — vital for retention and relationship building. |

### Key Analysis & Business Insights
**1. Which product line generates the most revenue, and why does it matter?**

**a. Product Line Performance Overview**

| Product Line     | Total Revenue | Total Orders  | Total Qty Ordered|  Total Customers |
|------------------|---------------|---------------|------------------|------------------|
| Classic Cars     | $3,919,615.66 | 967           | 33,992           | 89               |
| Motorcycles      | $1,166,388.34 | 331           | 11,663           | 49               |
| Planes           | $975,003.57   | 306           | 10,727           | 47               |
| Ships            | $714,437.13   | 234           | 8,127            | 48               |
| Trains           | $226,243.47   | 77            | 2,712            | 34               |
| Vintage Cars     | $1,903,150.84 | 607           | 21,069           | 84               |
| Trucks and Buses | $1,127,789.84 | 301           | 10777            | 48               |

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

**b. Total Sales Per Product Line in each year (2003–2005)**
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

**c. Percentage Contribution to Total Sales**

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

Segmenting revenue by deal size reveals how different types of customers contribute to overall performance. It helps shape pricing strategies, sales prioritization, and resource allocation.

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

**What the data shows:**
Medium deal sizes are the clear revenue leaders, contributing over 60% of total sales — making them the business’s backbone. Small deals come next, contributing around 26%, while Large deals surprisingly contribute the least, at just 13%.

**Business implications:**

- Focus efforts on nurturing the mid-market customer base, which delivers high volume and steady revenue.
- Use small deals to attract new customers and develop loyalty programs that encourage them to scale up.
- Reassess how large deals are approached — do they need better targeting, customized service, or better value propositions?

**Conclusion:**
Medium-sized transactions are your revenue engine. Understanding why large deals underperform could unlock untapped growth.


**3. Who are our most valuable customers, and how dependent are we on them?**

Understanding who your top customers are, where they’re located, and what they buy allows for smarter targeting, stronger retention strategies, and improved revenue forecasting. It also helps you tailor product offerings by region or segment.

**a. Top 10 Customers by Revenue**

**Why this matters**
In most businesses, a small segment of customers contributes a large share of revenue. Identifying and protecting these relationships is essential for revenue stability, forecasting, and retention strategy.

```sql
Select
      TOP 10 Customername,
Round
      (Sum(sales),2) As Total_revenue
From
      retail_sales_data_
Group by
      Customername
Order by
      Total_revenue DESC
```

| Rank | Customer Name                    | Revenue ($)    |
|------|----------------------------------|----------------|
| 1    | Euro Shopping Channel            | 912,294.11     |
| 2    | Mini Gifts Distributors Ltd.     | 654,858.06     |
| 3    | Australian Collectors, Co.       | 200,995.41     |
| 4    | Muscle Machine Inc               | 197,736.94     |
| 5    | La Rochelle Gifts                | 180,124.90     |
| 6    | Dragon Souveniers, Ltd.          | 172,989.68     |
| 7    | Land of Toys Inc.                | 164,069.44     |
| 8    | The Sharp Gifts Warehouse        | 160,010.27     |
| 9    | AV Stores, Co.                   | 157,807.81     |
| 10   | Anna's Decorations, Ltd          | 153,996.13     |

**Insight**
- The top customer alone, Euro Shopping Channel, contributed over $900K, making it an anchor account.
- Several other customers (e.g., Mini Gifts Distributors, Australian Collectors) also crossed the $150K+ threshold, showing repeat high-value behavior.
- Most of these clients have names suggesting corporate gift, souvenir, or collectible businesses — likely resellers or bulk buyers.

**Business Implications**
- These accounts should be treated like strategic partners, not just clients.
- Assign dedicated account managers, provide priority support, and offer early-access or bulk discounts.
- Analyze their product purchase patterns — they likely influence overall product performance data.

*Note: The top 20 customers together generated over $4.3 million, which is roughly 43% of total revenue — confirming the classic 80/20 principle, also known as the Pareto Principle, which states that roughly 80% of outcomes come from 20% of causes. i.e. 80% of revenue often comes from 20% of customers, 80% of sales are driven by 20% of products etc.*

**b. Total Number of Customers by Country**

The USA dominates, with 35 out of 77 total unique customers — that's ~45% of the entire customer base. France follows with 12 customers (~16%), while the remaining countries each have 5 or fewer. The bottom six countries (Norway, Finland, Canada, Germany, Italy, Spain) have minimal customer counts — only 3–5 each.

```sql
Select
      Country, 
Count
      (Distinct Customername) AS Total_Customers
From
      retail_sales_data_
Group by
      Country
Order by
      Total_Customers DESC
```

| Country   | Unique Customers |
|-----------|------------------|
| USA       | 35               |
| France    | 12               |
| Australia | 5                |
| UK        | 5                |
| Spain     | 5                |
| Norway    | 3                |
| Finland   | 3                |
| Canada    | 3                |
| Germany   | 3                |
| Italy     | 3                |

- **USA is your core market**
With nearly half of your customers, the USA is not just a high-revenue region — it's the backbone of your customer base. This concentration implies strong market fit, brand recognition, and likely repeat buyers.

  **Action:** Invest in localized marketing, fulfillment, and customer support here. Consider loyalty programs or exclusive offers for U.S. customers.

- **France is a high-potential secondary market**
France has a solid customer base — second only to the U.S. Its size suggests it could support more localized growth efforts.

  **Action:** Explore French-language support, geo-targeted promotions, or regional product preferences.

- **Australia, UK, and Spain are stable but underdeveloped**
These markets have similar small customer bases, which may reflect limited marketing, product awareness, or distribution challenges.

  **Action:** Identify friction points — pricing? shipping delays? lack of localized campaigns? — and test small marketing pushes.

- **Scattered low-volume countries: under-tapped or unfit?**
Countries like Germany, Canada, Italy, etc., show very low customer counts. They're either under-tapped (low exposure, not low interest) or the product doesn’t resonate culturally or logistically.

  **Action:** Conduct a quick country-by-country review on: Website traffic, Cart abandonment, Product reviews by region. Based on findings, decide whether to invest or maintain a passive presence.

**Conclusion**
The USA is your dominant customer stronghold, while France shows strong follow-up potential. Other countries show promise but require strategic exploration. A geo-segmented approach — focusing growth in France and Australia, while testing outreach in lower-engagement regions — can help balance customer acquisition and retention efforts globally.

**c. Total Customers per Product Line**

Classic Cars lead with 89 unique customers, closely followed by Vintage Cars (84), proving they are the most widely appealing product lines. There’s a noticeable drop-off to Motorcycles, Ships, Trucks and Buses, and Planes, all clustered between 47–49 customers. Trains lag significantly at 34 customers, making it the least engaged product line.

```sql
Select
      Productline, 
Count
      (Distinct Customername) AS Total_Customers
From
      retail_sales_data_
Group by
      Productline
Order by
      Total_Customers DESC
```
| Product Line       | Unique Customers |
|--------------------|------------------|
| Classic Cars       | 89               |
| Vintage Cars       | 84               |
| Motorcycles        | 49               |
| Ships              | 48               |
| Trucks and Buses   | 48               |
| Planes             | 47               |
| Trains             | 34               |

**Business Implications**
- Classic and Vintage Cars are not only top earners — they also have the widest customer reach. These should be at the center of retention, upsell, and bundling strategies.
- Mid-tier lines (Motorcycles, Ships, Trucks, Planes) show moderate interest. These may be under-promoted or have niche appeal. Segment these customers to discover specific traits or interests.
- Trains may be oversupplied or misaligned with customer preferences. It’s either a niche product or underperforming due to visibility, pricing, or design.

**Conclusion**
Customer count supports revenue trends: high customer engagement is concentrated in Classic and Vintage Cars, reinforcing their role as flagship lines. Mid-tier products show stable engagement, while Trains may need repositioning or refinement. Tailored strategies by product line can help maximize both reach and revenue.

**4. Which countries generate the most revenue — and how can we capitalize on them?**

Geographic performance analysis reveals where the business is thriving — and where growth opportunities may be untapped. It also guides logistics, customer support, and marketing localization.


```sql
Select
      Country, Year_Id,
Round
      (Sum (Sales), 2) as Total_Sales
From
      retail_sales_data_
Group by
      Country, Year_Id
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
Pivot (Sum(Total_Sales)
       For Year_Id
	   In ([2003], [2004], [2005])) as DD
```

| Country      | 2003        | 2004        | 2005        |
|--------------|-------------|-------------|-------------|
| Australia    | 253,134.45  | 232,396.68  | 145,091.97  |
| Austria      | 82,117.88   | 51,694.39   | 68,250.26   |
| Belgium      | 3,348.46    | 80,024.05   | 25,040.11   |
| Canada       | 54,609.50   | 135,776.09  | 33,692.97   |
| Denmark      | 99,192.72   | 120,431.56  | 26,012.87   |
| Finland      | 111,154.51  | 91,575.69   | 126,851.71  |
| France       | 312,761.42  | 555,198.70  | 242,956.40  |
| Germany      | 70,053.31   | 150,418.78  | –           |
| Ireland      | –           | 57,756.43   | –           |
| Italy        | 140,928.77  | 192,235.60  | 41,509.94   |
| Japan        | –           | 149,422.47  | 38,745.34   |
| Norway       | 196,532.60  | 110,931.10  | –           |
| Philippines  | 78,086.98   | 15,928.75   | –           |
| Singapore    | 165,686.20  | 116,039.03  | 6,763.18    |
| Spain        | 405,343.39  | 483,545.36  | 326,798.17  |
| Sweden       | 58,459.92   | 119,947.57  | 31,606.72   |
| Switzerland  | –           | 117,713.56  | –           |
| UK           | 180,421.55  | 257,656.10  | 40,802.81   |
| USA          | 1,305,147.88| 1,685,470.69| 637,364.26  |


- **Top Markets Overall (2003–2005 Combined):**
USA leads by far with over $3.6 million in revenue — nearly 3× higher than the next country. Spain follows, with a strong and steady performance each year, totaling over $1.2 million. France is a close third with over $1.1 million, showing peak revenue in 2004.

- **Mid-Tier Markets:**
UK, Italy, Singapore, and 🇦🇺 Australia each generated $500K–$700K total revenue. These countries show a mix of peaks and declines — worth investing in with localized campaigns or product alignment.

- **Low & Volatile Markets:**
Belgium, Philippines, Ireland, and Switzerland show inconsistent or low revenue with missing data in many years.

**Year-over-Year Trends:**
2004 was the strongest year across almost all markets, indicating company-wide growth, possibly due to product launches or global expansion. 2005 shows a general decline in most regions, especially the USA, Australia, and the UK — worth investigating for potential issues (economic? competitive? distribution?)

**Business Implications**
   - Double down on top-performing countries: USA, Spain, and France deserve continued focus with regionalized marketing, loyalty programs, and strong fulfillment pipelines.
   - Re-engage mid-tier markets: The UK, Italy, Australia, and Singapore show solid revenue but inconsistent growth — target them with reactivation campaigns or customer research.
   - Evaluate resource allocation in countries with low or missing data — Belgium, Ireland, Philippines. Either re-test these markets or shift focus elsewhere.

**Conclusion**
The USA, Spain, and France dominate in total revenue and should remain top strategic priorities. However, year-over-year fluctuations — especially the dip in 2005 — suggest a need for region-specific analysis and intervention. Investing in rising mid-tier markets while rationalizing underperforming regions can help the business maximize international performance.

**5. What does Month-over-Month revenue reveal about sales cycles?**

Month-over-Month (MoM) is a metric used to measure the percentage change in a value from one month to the next. It helps track short-term trends, fluctuations, and growth patterns in key business indicators like revenue, sales, or customer activity. By comparing each month to the previous one, businesses can quickly identify performance spikes, slowdowns, and seasonal patterns that inform decision-making.

| Month     | Revenue ($)   | Previous Month | MoM Change |
|-----------|----------------|----------------|------------|
| January   | 785,874.44     | –              | –          |
| February  | 810,441.90     | 785,874.44     | +3%        |
| March     | 754,501.39     | 810,441.90     | -7%        |
| April     | 669,390.96     | 754,501.39     | -11%       |
| May       | 923,972.56     | 669,390.96     | +38%       |
| June      | 454,756.78     | 923,972.56     | -51%       |
| July      | 514,875.97     | 454,756.78     | +13%       |
| August    | 659,310.57     | 514,875.97     | +28%       |
| September | 584,724.27     | 659,310.57     | -11%       |
| October   | 1,121,215.22   | 584,724.27     | +92%       |
| November  | 2,118,885.67   | 1,121,215.22   | +89%       |
| December  | 634,679.12     | 2,118,885.67   | -70%       |

**Insights:**
- Extreme peaks in: October (+92%), November (+89%). These two months are by far the strongest — likely due to holiday buying, end-of-year deals, or corporate bulk orders.
- Major dips in: June (–51%), December (–70%). December's drop is especially sharp after the November peak, suggesting either a cutoff in shipping cycles, budget closures, or reporting lag.
- Consistent activity in Q1 & Q3: Modest swings in Jan–April and July–September. Smaller growth and decline patterns suggest these are stable months with no heavy promotional spikes.

**Business Implications**
- Plan major promotions and launches in October and November — these months offer the best ROI due to natural demand surges.
- Watch for June and December dips — either re-strategize campaigns or use these months for stock replenishment, training, or backend upgrades.
- Q1 and Q3 show predictable patterns — ideal for baseline forecasting and sales team performance measurement.

**Conclusion**
MoM trends show a cyclical sales pattern, with explosive activity in October and November, and steep drops immediately before and after. This suggests a strong reliance on seasonal buying behavior. Aligning campaigns, logistics, and budget planning around these patterns can maximize revenue and efficiency year-round.

**6. What does Year-over-Year revenue reveal about sales cycles?**

Year-over-Year (YoY) compares performance for a metric — such as revenue — from one year to the same period in the previous year. It’s used to evaluate long-term trends, showing whether a business is growing, stable, or declining on an annual basis. 

| Year | Revenue ($)   | Previous Year | YoY Change |
|------|---------------|----------------|------------|
| 2003 | 3,516,979.55  | –              | –          |
| 2004 | 4,724,162.59  | 3,516,979.55   | +34%       |
| 2005 | 1,791,486.71  | 4,724,162.59   | -62%       |

**Insights:**
- 2004 saw strong growth, with a 34% increase in revenue compared to 2003. This indicates successful expansion, possibly due to wider market reach, product demand, or improved operations.
- 2005 dropped sharply by 62%, cutting annual revenue by more than half. This suggests major disruption — possibly in operations, demand, supply chain, or loss of key customers or regions.

**Business Implications**
- The growth in 2004 reflects strong business momentum — success strategies from that period should be studied and replicated.
- The steep decline in 2005 is a major concern. It may signal deeper issues such as:
  - Market saturation
  - Distribution problems
  - Product fatigue
  - Loss of key accounts
  - Global/regional events impacting sales

Next steps:
  - Investigate what changed between 2004 and 2005 (e.g., revenue by product line or country)
  - Assess operational, customer, or market factors
  - Build recovery strategies from lessons learned

**Conclusion**
Year-over-Year analysis reveals a healthy growth trend in 2004 followed by a severe contraction in 2005. While the initial increase signals business success, the sharp drop the following year highlights the need for strategic review and recovery planning.

### Recommendations
Based on the analysis of transaction-level historical sales data across product lines, customers, countries, and time dimensions, the following recommendations are proposed:

**1. Double Down on Top Performers**
- Classic Cars and Vintage Cars consistently dominate in revenue and customer engagement.
- Maintain strong inventory, exclusive releases, and promotional focus around these lines to protect and grow their market share.

**2. Nurture High-Potential Product Lines**
- Product lines like Motorcycles, Planes, and Trucks & Buses show stable customer interest but underperform in revenue.
- Use targeted marketing and bundling strategies to grow sales in these categories.

**3. Strengthen Strategic Customer Relationships**
- A small group of top customers (e.g., Euro Shopping Channel) accounts for a significant portion of revenue.
- Introduce loyalty programs, dedicated account management, and early-access offerings to retain and grow these high-value relationships.

**4. Focus on High-Performing Markets**
- USA, Spain, and France lead in both customer count and total revenue.
- Invest in localized marketing, faster shipping options, and region-specific offers to increase market share.

**5. Address Revenue Volatility**
- Revenue peaks in October–November, but drops sharply in December and June, indicating strong seasonality.
- Align promotions, staffing, and stock planning with these cycles to reduce waste and improve forecasting.

**6. Investigate YoY Decline**
- While 2004 showed 34% YoY growth, revenue fell by 62% in 2005.
- Investigate causes (e.g., customer churn, product issues, economic conditions) and apply recovery strategies based on 2004’s success drivers.

### Conclusion 
This SQL-based sales analysis provided actionable insights into how revenue is distributed across products, customers, regions, and time. Key drivers of performance were identified — including dominant product lines, strategic customers, and top-performing countries.

Patterns such as monthly seasonality and annual volatility revealed areas for operational improvement and growth. The data confirms that the business is supported by a concentrated set of high-value customers and products, with untapped potential in mid-tier categories and secondary markets.

With the right mix of retention strategies, localized expansion, and seasonal optimization, the company can scale efficiently while reducing risk and stabilizing long-term revenue performance.

### Contact

**Name:** Obianujunwa Vivian John

**Email:** ujunwajohn01@gmail.com

**Location:** Lagos, Nigeria

**Role Target:** Data Analyst 

### THANK YOU!
