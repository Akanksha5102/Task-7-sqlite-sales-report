# Task-7-sqlite-sales-report
This project demonstrates how to create, analyze, and visualize sales data using **SQLite** for storage, **Pandas** for data manipulation, and **Matplotlib** for visualization.

# Sales Data Analysis with SQLite, Pandas, and Matplotlib

This project demonstrates how to create, analyze, and visualize sales data using **SQLite** for storage, **Pandas** for data manipulation, and **Matplotlib** for visualization.



## Features

- Create and populate a SQLite database with sample sales data
- Summarize total quantity sold and revenue per product
- Generate bar charts of revenue by product
- Export results to a PNG chart file


## Project Structure

1. ***Create the database and insert data:***

```
import sqlite3

conn = sqlite3.connect("sales_data.db")
cursor = conn.cursor()
cursor.execute("""
create table if not exists sales (
    product text,
    quantity integer,
    price real
)
""")

sample_data = [
    ('Product A', 10, 5.0),
    ('Product B', 20, 8.0),
    ('Product A', 5, 5.0),
    ('Product C', 7, 12.0),
    ('Product B', 3, 8.0),
    ('Product C', 2, 12.0)
]
cursor.executemany("INSERT INTO sales (product, quantity, price) VALUES (?, ?, ?)", sample_data)

conn.commit()
conn.close()
```

2. ***Summarize sales data:***

```
import sqlite3
import pandas as pd
conn = sqlite3.connect("sales_data.db")
query = """
select
    product, 
    sum(quantity) AS total_quantity, 
    sum(quantity * price) AS total_revenue
from sales
group by product
"""
df = pd.read_sql_query(query, conn)

print(df)
conn.close()
```
3. ***Generate revenue bar chart:***

```
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt

conn = sqlite3.connect("sales_data.db")
query = """
SELECT 
    product, 
    SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY product
"""

df = pd.read_sql_query(query, conn)
conn.close()

plt.figure(figsize=(6,4))
plt.bar(df['product'], df['total_revenue'], color='skyblue')
plt.title("Revenue by Product")
plt.xlabel("Product")
plt.ylabel("Total Revenue")
plt.tight_layout()
plt.savefig("sales_chart.png")
plt.show()
```

## Output Screen

