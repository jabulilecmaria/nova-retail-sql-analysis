
# SQL Sales Analysis Project 📊

## Project Overview

This project analyses sales data using SQL in Databricks to identify trends in sales performance, customer behaviour, profitability, discount effectiveness, and product performance.

The analysis was designed to transform transactional data into meaningful business insights that can support management decision-making.

## Business Objectives

The project focuses on:

- Analysing overall sales and profitability
- Understanding customer purchasing behaviour
- Evaluating regional and product performance
- Analysing the relationship between customer satisfaction and repeat purchases
- Assessing the effectiveness of different discount levels
- Identifying products that may require improved positioning or promotion
- Developing data-driven business recommendations

## Tools & Technologies

- **SQL**
- **Databricks**
- **GitHub**
- **ERD / Relational Database Design**

## Database Structure

The analysis uses a relational database consisting of:

- Customers
- Products
- Sales
- Customer Feedback
- Suppliers

The relationships between the tables were analysed using primary and foreign keys.

### Entity Relationship Diagram

![ERD](ERD.png)

## SQL Analysis

The project covers several levels of SQL analysis:

### Basic SQL
- Filtering and sorting data
- Product catalogue analysis
- Aggregations
- Revenue calculations

### Intermediate SQL
- JOIN operations
- Customer and product analysis
- GROUP BY and HAVING
- Subqueries and conditional logic

### Advanced SQL
- Common Table Expressions (CTEs)
- Window functions
- Ranking
- Customer and regional analysis
- Profit margin calculations

### Business Intelligence
- Customer satisfaction vs. repeat purchasing
- Discount effectiveness
- Product portfolio optimisation

## Key Business Insights

### 1. Customer Satisfaction vs Repeat Purchases

The analysis showed that highly satisfied customers were more numerous, with **252 customers**, compared with **185 less-satisfied customers**.

However, less-satisfied customers had a slightly higher average number of orders per customer (**5.54**) compared with highly satisfied customers (**5.04**).

This indicates that purchase frequency does not necessarily correspond directly with customer satisfaction. Management should investigate why frequent customers continue purchasing despite reporting lower satisfaction.

### 2. Discount Effectiveness

The analysis identified a clear relationship between increasing discount levels and declining profit margins.

| Discount Band | Profit Margin |
|---|---:|
| 0% | 39.98% |
| 1–10% | 35.36% |
| 11–20% | 27.46% |
| 21–30% | 18.62% |

The results indicate that aggressive discounting can significantly reduce profitability. Management should therefore use higher discounts selectively and monitor whether the additional sales generated justify the reduction in margin.

### 3. Product Portfolio Performance

Several lower-profit products maintained relatively healthy profit margins of approximately 34%–36%, suggesting that their weaker performance was more closely associated with lower sales volume than poor profitability.

Products such as the Electric Kettle may therefore benefit from targeted promotion, bundling, or improved positioning before being considered for discontinuation.

## Strategic Recommendations

Based on the analysis, the following actions are recommended:

1. Investigate the causes of dissatisfaction among frequent customers and use customer feedback to improve the customer experience.

2. Prioritise lower discount levels where possible and evaluate the profitability of high-discount promotional campaigns.

3. Use targeted promotions and product bundling to improve sales volumes for selected underperforming products.

4. Monitor product-level revenue, profit and margin to support better portfolio decisions.

5. Continue using SQL-based analysis to monitor business performance and support data-driven decision-making.

## Repository Contents

| File | Description |
|---|---|
| `SQL_Sales_Analysis_Project.sql` | SQL queries used for the analysis |
| `ERD.png` | Entity Relationship Diagram |
| `Management_Report.pdf` | Management analysis and business recommendations |

## Skills Demonstrated

- SQL querying
- Data aggregation
- JOINs
- CTEs
- Window functions
- Ranking
- Conditional logic
- Profitability analysis
- Customer behaviour analysis
- Business intelligence
- Data-driven decision-making
- Relational database analysis
