```py
import pandas as pd

# create a table with two columns
cars = pd.Series(['BMW', 'Toyota', 'Honda'])
colors = pd.Series(['Red', 'Blue', 'White'])
car_data = pd.DataFrame({'Car': cars, 'Color': colors})

# read, write
car_sales = pd.read_csv('./car-sales.csv')
car_sales.to_csv('exported-car-sales.csv', index=False)
car_sales.dtypes    # data types of columns
car_sales.columns
len(car_sales)     # get number of rows
car_sales['Doors'].sum()    # sum of all numbers in doors column
car_sales.head(7)    # view top 7 rows
car_sales.tail(7)    # view last 7 rows
car_sales.loc[3]     # view row at index 3
car_sales.iloc[3]    # view row at position 3, refers to line-number
```
