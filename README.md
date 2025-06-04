# Supermart_Grocery_Sales_Report

# Import data to SQL database
* Prepare a csv file
* Create table in SQL
* Import csv file into sql

# ETL(Extract Transform and Load) Process
1. ------------Extract:---------------------
* Extract the data from the source system as a csv file into a SQL Database
* Ensure the data is loaded correctly into SQL Table with appropriate initial schema.

2. -----------Transform:-------------------------
* Data Cleaning
   1. Check duplicate values and drop it if duplicates are found.
    * I am mentioning the queries which I have used in this project.
      
    - QUERIES for Duplicate values (Unique order_id and all the rest columnn)
      * SELECT Order_Id, Customer_Name, Category, Sub_category, City,
        Order_Date, Region, Sales, Discount, Profit, State,
        COUNT(*) AS duplicate_count FROM supermart_grocery_sales_details
        GROUP BY Order_Id, Customer_Name, Category, Sub_category, City,
          Order_Date, Region, Sales, Discount, Profit, State
        HAVING COUNT(*) > 1;
        
     - QUERIES for drop the duplicate values 
      * DELETE * FROM supermart_grocery_sales_details
       WHERE Order_Id NOT IN (
                                SELECT MIN(Order_Id)
                                FROM supermart_grocery_sales_details
                                GROUP BY Customer_Name, Category, Sub_category, City, Order_Date, Region, Sales, Discount, Profit, State
                               );
  2. Check NULL or Missing values
     - QUERIES for finding NULL or Missing values
        select *,count(*) as null_count from supermart_grocery_sales_details 
         where Order_Id is null
         or Customer_Name is null
         or Category is null
         or Sub_category is null
         or city is null
         or Order_Date is null 
         or Region is null
         or Sales is null
         or Discount is null
         or Profit is null
         or State is null
       
  3. Check for invalid or future date
     - Queries for finding Invalid or future date
            select Order_Date, count(*)as Invalid_Date
            from supermart_grocery_sales_details where Order_Date > curDate();
  4. Check for outliers or unusual values
     - Queries for outliers or unusual values
            select *, count(*)as Invalid_values
            from supermart_grocery_sales_details where sales<0 or profit<0;
  
* Data Enrichment
- After cleaning connect SQL Database to Power BI for advanced analytics
- Create new calculated columns by using DAX Queries in PowerBI based on business login and domain understanding

  * DAX Queries
     1. QTR = FORMAT(supermart_grocery_sales_report[Order_Date].[Quarter],"QTR") -> This queries give me as text format
     2. Order_Year = YEAR('supermart_grocery_sales_report'[Order_Date]) - I used this instead of Format() bcz it will return numeric value.
     3. Current _Year_Profit = CALCULATE(SUM(supermart_grocery_sales_report[Profit]),
                       FILTER(ALL(supermart_grocery_sales_report),
                       supermart_grocery_sales_report[Order_Year]=MAX(supermart_grocery_sales_report[Order_Year])))
     4. Current_Year_Sales = CALCULATE(SUM(supermart_grocery_sales_report[Sales]),
                            FILTER(all(supermart_grocery_sales_report),
                            supermart_grocery_sales_report[Order_Year]=MAX(supermart_grocery_sales_report[Order_Year])))
     5. Previous_Year_Profit = CALCULATE(SUM(supermart_grocery_sales_report[Profit]),
                        FILTER(ALL(supermart_grocery_sales_report),
                        supermart_grocery_sales_report[Order_Year]=MAX(supermart_grocery_sales_report[Order_Year])-1))
     6. Previous_Year_Sales = CALCULATE(SUM(supermart_grocery_sales_report[Sales]),
                             FILTER(ALL(supermart_grocery_sales_report),
                               supermart_grocery_sales_report[Order_Year]=MAX(supermart_grocery_sales_report[Order_Year])-1))
     7. YOY_Profit_inpercentage = DIVIDE(supermart_grocery_sales_report[Current _Year_Profit]- 
                       supermart_grocery_sales_report[Previous_Year_Profit],supermart_grocery_sales_report[Previous_Year_Profit])
     8. YOY_Sales_in_percentage = DIVIDE(supermart_grocery_sales_report[Current_Year_Sales]- 
                      supermart_grocery_sales_report[Previous_Year_Sales],supermart_grocery_sales_report[Previous_Year_Sales])
     9. Avg_Discount = AVERAGE(supermart_grocery_sales_report[Discount])

3. ------------Load:-------------------
   * Load the transformed and enriched data into Power BI dashboards or reports for visualization and decision-making.

* These steps are part of the data cleaning phase in the ETL pipeline.

# PROJECT INSIGHTS - YEAR 2018 ---------------------------------------------------------------------------------

* YOY(Year on Year) change :-
  1. Sales increased by 28.55%
  2. Profit increased by 30.52%

* Overview YTD
  1. Total Sales 15 M
  2. Total Profit 3.75 M
  3. The average discount across all products is 22.68%.
  4. Sales and profit in 2018 increased by 28.55% and 30.52%, respectively.
  5. The Snacks category contributed the highest profit among all categories, totaling $0.14M.
  6. Health Drinks and Soft Drinks sub-categories contributed the most in both sales ($1.05M and $1.03M) and profit ($0.27M and $0.26M).
  7. The West region contributed the highest in sales, accounting for 32% of the total.
  8. The Beverages category had the highest average discount, at 15%.
  9. The Eggs, Meat & Fish and Snacks categories contributed significantly to sales, at $2.3M and $2.2M, respectively.
  10. Kanyakumari city recorded the highest contribution to sales, totaling $0.71M.

# Sales forcasting for next 5 years (2019,2020,2021,2022 & 2023)
* This report page presents a Year-over-Year (YOY) sales trend along with forecasting for the next five years based on historical data.
Key Highlights :-
1. Historical Sales growth (2015-2018)
 => Sales has grown consistently from 0.9M in 2015 to 1.5M in 2018, showing a strong upward trend in revenue
2. Forcast in 2023
=> Predicted sales :- 8.31M
1. Upper bond :- 9.03M
2. Lower bond :- 7.59M
 This predicted sales based on past trend.
      
* Marketing Strategy Summary ---------------------------------------------------------------------------------
1. Boost High-Profit Categories
   Focus promotions on Snacks, Health Drinks, and Soft Drinks – they drive the most profit and sales.
2. Target West Region
   Run geo-targeted ads and local offers in the West, which contributes 32% of sales.
3. City-Specific Campaign – Kanyakumari
   Launch exclusive city offers in Kanyakumari, your top-performing city.
4. Optimize Discounts
   Reduce overall discounts (avg. 22.68%) and focus on targeted promotions, especially in Beverages and Egg, Meat & Fish. This stategy
   can maximise margin and boost profit.
6. Plan for Forecasted Growth
   Use forecast data (up to 8.31M sales) to scale inventory and plan campaigns ahead of demand spikes.
7. Product Line Expansion
   Expand top-performing sub-categories like Health & Soft Drinks to increase basket size.
8. Leverage Personalization
   Use customer data for email targeting, retargeting, and personalized offers across channels.
