# Step-by-Step Demonstration of Connecting Python to PostgresSQL Database
Step 1 : Loading of Neccesary Libraries
import pandas as pd
import psycopg2
Step 2 : Loading of DataFrame (Sales_table -an excel files)
import_File = 'Sales_table.xlsx'
df = pd.read_excel(import_File)
df.head()
id	Order_Date	Region	Country	Item_type	Quantity	Price
0	1	2011-04-04	Australia	Australia	Meat	4300	421.89
1	2	2018-07-12	Asia	Tajikistan	Personal Care	4145	81.73
2	3	2011-07-06	Saharan Africa	Mozambique	Cosmetics	6407	437.20
3	4	2011-05-01	Central America	Panama	Personal Care	2810	81.73
4	5	2013-11-15	North America	Canada	Fruits	2110	9.33
Step 3 : Setting the connection parameters
conn = psycopg2.connect(
    host = 'localhost',
    database ='customersdb',
    user ='postgres',
    password ='giles'
)
cursor = conn.cursor()
Step 4 : Creating a table named Sales_table in PostgreSQL using an SQL query executed through Python. The table is defined within the Python script and is subsequently created in the connected PostgreSQL database."
Table_query = """
CREATE TABLE  Sales_table(
    custid      INT PRIMARY KEY,
    Order_Date  DATE,
    Region      VARCHAR(100),
    Country     VARCHAR(100),
    Item_type   VARCHAR(100),
    Quantity    INT,
    Price       DECIMAL(10, 2)
)
"""
cursor.execute(Table_query) 
conn.commit()
Checking the tale columns
print(df.columns)
Index(['id', 'Order_Date', 'Region', 'Country', 'Item_type', 'Quantity',
       ' Price'],
      dtype='object')
Step 5: Converting the values in the "Order_Date" column of the DataFrame from string format to Python datetime objects, using the date format "%y/%m/%d" (year/month/day).
df["Order_Date"] = pd.to_datetime(df["Order_Date"], format="%y/%m/%d")
Step 6: Introduce tuples that hold a list of all the rows in the DataFrame [df] as regular Python tuples
Tuples = list(df.itertuples(index=False, name=None))
Step 7: Inserting multiple rows of sales data into the Sales_table in the PostgreSQL database by executing a parameterized SQL INSERT query using the values stored in the Tuples list.
insert_query = """
INSERT INTO Sales_table(custid, Order_Date, Region, Country, Item_type, Quantity, Price)
VALUES (%s, %s, %s, %s, %s, %s, %s)
"""
cursor.executemany(insert_query, Tuples)
conn.commit()
Step 8: Closing the cursor and Postgres database connection
    cursor.close()
    conn.close()
