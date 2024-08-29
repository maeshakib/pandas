# pandas

```python
import pandas as pd


```
### Creating data
There are two core objects in pandas: the DataFrame and the Series.

### DataFrame
A DataFrame is a table. It contains an array of individual entries, each of which has a certain value. Each entry corresponds to a row (or record) and a column.

For example, consider the following simple DataFrame:

```python
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
print(df)
```
- In this example, the "0, No" entry has the value of 131. The "0, Yes" entry has a value of 50, and so on.
- DataFrame entries are not limited to integers. For instance, here's a DataFrame whose values are strings:
- We are using the pd.DataFrame() constructor to generate these DataFrame objects.
- The syntax for declaring a new one is a dictionary whose keys are the column names (Bob and Sue in this example), and whose values are a list of entries. This is the standard way of constructing a new DataFrame, and the one you are most likely to encounter
- The dictionary-list constructor assigns values to the column labels, but just uses an ascending count from 0 (0, 1, 2, 3, ...) for the row labels. Sometimes this is OK, but oftentimes we will want to assign these labels ourselves.
- The list of row labels used in a DataFrame is known as an Index. We can assign values to it by using an index parameter in our constructor:


### Series
- A Series is, in essence, a single column of a DataFrame. So you can assign row labels to the Series the same way as before, using an index parameter. However, a Series does not have a column name, it only has one overall name:
```python
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```

- The Series and the DataFrame are intimately related. It's helpful to think of a DataFrame as actually being just a bunch of Series "glued together". We'll see more of this in the next section of this tutorial.
- 
### Reading data files
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")

