# Step-by-Step Demonstration of Connecting Python to a PostgreSQL Database

## Step 1: Loading the Necessary Libraries
```python
import pandas as pd
import psycopg2
## Step 2: Loading the Excel File into a DataFrame (Sales_table.xlsx)
import_File = 'Sales_table.xlsx'
df = pd.read_excel(import_File)
df.head()
