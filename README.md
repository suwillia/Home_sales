# Home Sales

This project uses Apache Spark to query housing data in SQL format. The project involves importing a CSV from a URL as a DataFrame, then performing various SQL queries to analyze home sales data.

## Project Overview

The analysis focuses on answering the following questions:

1. What is the average price for a four-bedroom house sold for each year? (Rounded to two decimal places)
2. What is the average price of a home for each year the home was built, that has three bedrooms and three bathrooms? (Rounded to two decimal places)
3. What is the average price of a home for each year the home was built, that has three bedrooms, three bathrooms, two floors, and is greater than or equal to 2,000 square feet? (Rounded to two decimal places)
4. What is the average price of a home per "view" rating having an average home price greater than or equal to $350,000? (Rounded to two decimal places)

## Performance Optimization

To improve query performance, we:
- Cached the table to speed up repeated queries
- Created partitioned parquet files based on the "date_built" column
- Compared query runtimes before and after these optimizations

## Implementation Details

### Sample Query

```python
start_time = time.time()
query = """
(SELECT round(avg(price),2) as avg_price, view 
from home_sales
group by view
having avg(price) >= 350000
order by view desc)
"""
spark.sql(query).show()

print("--- %s seconds ---" % (time.time() - start_time))

```
### Partitioning Implementation
```
Partition by the "date_built" field on the formatted parquet home sales data 
sparkSQL.write.partitionBy('date_built').mode('overwrite').parquet('homes_parquet')
'''

### Results
Cached tables showed marked speed increases while parquet data was faster that original query, it was still slower than the cached version.

