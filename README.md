# Retail Sales Analysis SQL Project

**Project Title**: Retail Sales Analysis  
**Database**: `sql_p1`

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_p1`.
- **Table Creation: A table named retail_sales is created to store retail sales data. The table structure includes columns for transaction ID (transactions_id), sale date (sale_date), sale time (sale_time), customer ID (customer_id), gender (gender), age (age), product category (category), quantity sold (quantiy), price per unit (price_per_unit), cost of goods sold (cogs), and total sale amount (total_sale).**

```sql
CREATE DATABASE sql_p1;

create table retail_sales(
transactions_id serial primary key,
sale_date date,
sale_time time,
customer_id int,
gender varchar(15),
age int,
category varchar(15),
quantiy int,
price_per_unit float,
cogs float,
total_sale float
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
--Data Exploration
--how many sales we have?
select count(*) as total_sales from retail_sales;
--how many costumers we have?
select count(distinct customer_id) as total_customers from retail_sales;
--how many categories we have?
select distinct category from retail_sales;
select count(distinct category) from retail_sales;
select count( category) from retail_sales;

--Data cleaning
select * from retail_sales
where
transactions_id is null
or
sale_date is null
or
sale_time is null
or
customer_id is null
or
gender is null
or
age is null
or
category is null
or
quantiy is null
or
price_per_unit is null
or
cogs is null
or
total_sale is null;
--
delete from retail_sales
where
transactions_id is null
or
sale_date is null
or
sale_time is null
or
customer_id is null
or
gender is null
or
age is null
or
category is null
or
quantiy is null
or
price_per_unit is null
or
cogs is null
or
total_sale is null;

alter table retail_sales
rename column quantiy to quantity;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

**Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'**
select * from retail_sales
where sale_date='2022-11-05';

**Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022**
select * from retail_sales
where category= 'Clothing' and quantity>3 and sale_date>='2022-11-01' and sale_date<'2022-12-01';

**Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.**
select category, sum(total_sale) as net_sale 
	from retail_sales
	group by category;
	
**Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**
select round(avg(age)) from retail_sales
where category='Beauty';

**Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.**
select * from retail_sales
where total_sale>1000;

**Q.6 Write a SQL query to find the total number of transactions (transactions_id) made by each gender in each category.**
select count(transactions_id), gender, category from retail_sales
group by gender, category;

**Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.**
select* from(
select
	extract(year from sale_date) as year,
	extract(month from sale_date)as month,
	avg(total_sale) as avg_sale,
	rank() over(partition by extract(year from sale_date) order by avg(total_sale) desc ) as rank
	from retail_sales
	group by year, month)
	where rank=1;

**Q.8 Write a SQL query to find the top 5 customers based on the highest total sales**
select customer_id,
	sum(total_sale) as total_sale
	from retail_sales
	group by 1
	order by 2 desc
	limit 5;

**Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.**
select count(distinct customer_id), category
from retail_sales
group by category;

**Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17).**
select
case when sale_time<='12:00:00' then 'morning'
	 when sale_time>'12:00:00' and sale_time<='17:00:00' then 'afternoon'
	 else 'evening'
	 end as shift,
	 count(transactions_id) as total_orders
	 from retail_sales
	 group by shift
	 order by total_orders;

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports & Analysis:

### Sales Analysis
- Retrieved sales transactions for specific dates.
- Calculated total sales by product category.
- Identified high-value transactions with total sales above 1000.
- Analyzed monthly average sales and identified the best-selling month for each year.

### Customer Analysis
- Identified the top 5 customers based on total sales.
- Calculated the number of unique customers purchasing from each category.
- Analyzed customer demographics through gender and age-based analysis.

### Order & Time Analysis
- Analyzed transaction volume by gender and category.
- Classified transactions into Morning, Afternoon, and Evening shifts.
- Compared order volumes across different shifts.


## Author - sahil yadav

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!


- **LinkedIn**: [Connect with me professionally](www.linkedin.com/in/sahil-yadav-bbb1bb3b1)

