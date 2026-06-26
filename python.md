## Import libraries
```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns

from sqlalchemy import create_engine

%matplotlib inline
```

## Connect to MySQL Database
```python
USER = "root"
PASSWORD = "your_password"
HOST = "localhost"
PORT = "3306"
DATABASE = "hotel_booking"

connection_string = f"mysql+pymysql://{USER}:{PASSWORD}@{HOST}:{PORT}/{DATABASE}"

engine = create_engine(connection_string)
```

## Load dataset

```python
query = "SELECT * FROM hotel_booking;"
df = pd.read_sql(query, con=engine)
print(f"Successfully loaded {df.shape[0]} rows and {df.shape[1]} columns!")
```
## Preview Dataset
```python
df.head()
```

## Dataset Information
```python
df.info()
```

## Summary statistics
```python
df.describe()
```

## Exploratory Data Analysis

### Distribution of Booking Lead Time
```python
plt.figure(figsize=(10,5))

sns.histplot(
    data=df,
    x='lead_time',
    bins=50,
    kde=True,
    color='green'
)

plt.title('Distribution of Booking Lead Time')
plt.xlabel('Lead Time (Days)')
plt.ylabel('Number of Bookings')
plt.savefig('../Images/Distribution_of_leadtime.png',
            dpi=300,
            bbox_inches='tight')
plt.show()
```

