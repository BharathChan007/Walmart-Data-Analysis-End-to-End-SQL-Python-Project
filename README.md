# Walmart-Data-Analysis-End-to-End-SQL-Python-Project
![image](https://github.com/user-attachments/assets/5647809f-419b-4c73-94c2-b56918d8511a)

This project is an end-to-end data analysis solution designed to extract critical business insights from Walmart sales data. We utilize Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques to solve key business questions
----------------------------------------------------------------------------------------------------------------------------
## Project Steps
# 1. Set Up the Environment
Tools Used: Visual Studio Code (VS Code), Python, SQL (PostgreSQL)
**Objective:** Create a well-organized workspace within VS Code to streamline development and data handling.

# 2. Configure Kaggle API for Dataset Access
**API Setup:**
To access datasets from Kaggle directly, I set up the Kaggle API by obtaining the API token (JSON file) from my Kaggle profile settings.

Configuration Steps:
Placed the kaggle.json file in the .kaggle folder.
Used the command kaggle datasets download -d <dataset-path> to download datasets into my project folder.

# 3. Download Walmart Sales Data
**Data Source**: Walmart Sales Dataset from Kaggle
**Storage:** Saved the downloaded data files in a data/ folder for easy access and organization.

# 4. Install Required Libraries and Load Data
**Libraries:** Installed essential Python libraries for data manipulation and database connection:

bash
Copy code
pip install pandas numpy sqlalchemy mysql-connector-python psycopg2
Loading Data: Loaded the Walmart sales data into a Pandas DataFrame for preliminary analysis and transformation.

# 5. Data Exploration
**Goal:** Perform initial data exploration to gain an understanding of data distribution, column names, types, and identify potential issues.

**Exploration Techniques**:  Used .info(), .describe(), and .head() to review data structure, statistics, and get a general sense of the data quality.

# *6. Data Cleaning*
**Steps Taken:**

Remove Duplicates: Identified and removed duplicate entries to ensure accuracy in analysis.
Handle Missing Values: Filled or removed missing values based on significance.
Fix Data Types: Ensured consistency in data types (e.g., dates formatted as datetime, prices as float).
Currency Formatting: Used .replace() to handle currency symbols, ensuring numeric values for analysis.
Validation: Checked for remaining inconsistencies and verified data integrity after cleaning.

# 7. Feature Engineering
**Objective:** Enhance the dataset with additional metrics for more detailed analysis.

Created a new column, Total Amount, by calculating the product of unit_price and quantity for each transaction. This calculated field is useful for further SQL aggregation and analysis.

# 8. Load Data into PostgreSQL
**Database Connections:** Established connections to PostgreSQL using psycog2 and loaded the cleaned data into each database.

**Table Creation:**

Automated table creation and data insertion in PostgreSQL to streamline the process.
**Verification:** Executed initial SQL queries to confirm that data was loaded accurately in both databases.

# 9. SQL Analysis: Complex Queries and Business Insights
**Objective:** Address critical business questions using SQL to reveal insights.

 Finding different payment method and number of transacations, number of qty sold
 ```sql
SELECT DISTINCT payment_method, count(*) as num_trans, sum(quantity) as qty_sld
 FROM walmart
 GROUP BY payment_method
```
Identify the highest-rated category in each branch, displaying the branch and category (Average Rating)
```sql SELECT *
FROM (
	SELECT  branch, category, avg(rating), rank() over(partition by branch order by avg(rating) desc) as rank 
	FROM walmart
	group by branch, category
)
WHERE rank = 1;
```

Identifying the busiest day for each branch based on the number of transactions
```sqlSELECT *
FROM
	(SELECT branch, 
		TO_CHAR(TO_DATE(date, 'DD/MM/YYY'), 'Day') as day_name,
		COUNT(*) as no_trans,
		RANK() OVER(PARTITION BY branch ORDER BY count(*) DESC) as rank
	FROM walmart
	GROUP BY 1,2
)
WHERE rank =1;
```
Determining the average, min and max rating of products for each city. Listing the city, avg_rating, min_rating and max_rating
```sql
SELECT city, category, AVG(rating), MIN(rating), MAX(rating)
FROM walmart
GROUP BY 1, 2
ORDER BY 3 DESC;
```
Calculating the total profit for each category by considering total_profit as (unit_price * quantity * profit_margin). Listing category and total_profit, ordered from highest to lowest
```sql
SELECT category, SUM(total * profit_margin) as total_profit
FROM walmart
GROUP BY 1
ORDER BY 2 DESC
```
Determining the most common payment method for each branch. Displaying branch and the preffered_payment_method
```sql
SELECT *
FROM 
	(SELECT branch, 
			payment_method,
			COUNT(*) as total_trans,
			RANK() OVER (PARTITION BY branch ORDER BY COUNT(*) DESC) as rank
	FROM walmart
	GROUP BY 1, 2
)
WHERE rank = 1;

--- Q.7  Categorize sales into 3 group numbers MORNING, AFTERNOON, EVENING
----      Find out which of the shift and no. of invoices

SELECT branch,
	CASE 
		WHEN EXTRACT (HOUR FROM (time::time)) < 12 THEN 'Morning'
		WHEN EXTRACT (HOUR FROM (time::time)) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END day_time,
	COUNT(*)
FROM walmart
GROUP BY 1,2
ORDER BY 1, 3 DESC
```
Identifying top 5 branch with highest decrease ratio in revenue compare to last year (current year 2023 and last year 2022)
```sql
WITH revenue_2022
AS
(
	SELECT branch,
		sum(total) as revenue
	FROM walmart
	WHERE EXTRACT(YEAR FROM TO_DATE(date, 'DD/MM/YY')) = 2022
	GROUP BY 1
),
revenue_2023
AS
(
	SELECT branch,
		sum(total) as revenue
	FROM walmart
	WHERE EXTRACT(YEAR FROM TO_DATE(date, 'DD/MM/YY')) = 2023
	GROUP BY 1
)
SELECT 
	ls.branch,
	ls.revenue as last_year_revenue,
	cs.revenue as cr_year_revenue,
	ROUND((ls.revenue - cs.revenue)::numeric/ls.revenue::numeric*100, 2) AS rev_dec_ratio
FROM revenue_2022 as ls
JOIN
revenue_2023 as cs
ON ls.branch = cs.branch
WHERE ls.revenue > cs.revenue
ORDER BY 4 DESC
LIMIT 5
```
