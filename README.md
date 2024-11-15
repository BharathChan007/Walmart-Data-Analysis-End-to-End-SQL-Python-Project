# Walmart-Data-Analysis-End-to-End-SQL-Python-Project
![image](https://github.com/user-attachments/assets/5647809f-419b-4c73-94c2-b56918d8511a)

This project is an end-to-end data analysis solution designed to extract critical business insights from Walmart sales data. We utilize Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques to solve key business questions
----------------------------------------------------------------------------------------------------------------------------
## Project Steps
# 1. Set Up the Environment
*Tools Used:* Visual Studio Code (VS Code), Python, SQL (PostgreSQL)
*Objective:* Create a well-organized workspace within VS Code to streamline development and data handling.

#2. Configure Kaggle API for Dataset Access
*API Setup:*
To access datasets from Kaggle directly, I set up the Kaggle API by obtaining the API token (JSON file) from my Kaggle profile settings.

Configuration Steps:
Placed the kaggle.json file in the .kaggle folder.
Used the command kaggle datasets download -d <dataset-path> to download datasets into my project folder.

#3. Download Walmart Sales Data
Data Source: Walmart Sales Dataset from Kaggle
*Storage:* Saved the downloaded data files in a data/ folder for easy access and organization.

#4. Install Required Libraries and Load Data
*Libraries:* Installed essential Python libraries for data manipulation and database connection:

bash
Copy code
pip install pandas numpy sqlalchemy mysql-connector-python psycopg2
Loading Data: Loaded the Walmart sales data into a Pandas DataFrame for preliminary analysis and transformation.

#5. Data Exploration
Goal: Perform initial data exploration to gain an understanding of data distribution, column names, types, and identify potential issues.

Exploration Techniques:Used .info(), .describe(), and .head() to review data structure, statistics, and get a general sense of the data quality.

#*6. Data Cleaning*
Steps Taken:

Remove Duplicates: Identified and removed duplicate entries to ensure accuracy in analysis.
Handle Missing Values: Filled or removed missing values based on significance.
Fix Data Types: Ensured consistency in data types (e.g., dates formatted as datetime, prices as float).
Currency Formatting: Used .replace() to handle currency symbols, ensuring numeric values for analysis.
Validation: Checked for remaining inconsistencies and verified data integrity after cleaning.

#7. Feature Engineering
Objective: Enhance the dataset with additional metrics for more detailed analysis.

Created a new column, Total Amount, by calculating the product of unit_price and quantity for each transaction. This calculated field is useful for further SQL aggregation and analysis.

#8. Load Data into PostgreSQL
Database Connections: Established connections to PostgreSQL using psycog2 and loaded the cleaned data into each database.

Table Creation:

Automated table creation and data insertion in both MySQL and PostgreSQL to streamline the process.
Verification: Executed initial SQL queries to confirm that data was loaded accurately in both databases.

#9. SQL Analysis: Complex Queries and Business Insights
Objective: Address critical business questions using SQL to reveal insights.

Revenue Trends: Analyzed revenue across different branches and product categories.
Best-Selling Products: Identified top-selling product categories.
Sales Performance by Time and Location: Examined sales performance by city, time, and payment method.
Peak Sales Periods: Analyzed seasonal or periodic sales spikes to identify peak sales periods.
Profit Margin Analysis: Calculated profit margins by branch and category.
Documentation: Documented each query's purpose, approach, and results, providing a clear understanding of the analysis process.
