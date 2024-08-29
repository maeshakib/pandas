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

wine_reviews.head()
- see  contents of the resultant DataFrame using the head() command, which grabs the first five rows

#  Exercise pandas: Creating, Reading and Writing

```python
import pandas as pd
pd.set_option('display.max_rows', 5)
from learntools.core import binder; binder.bind(globals())
from learntools.pandas.creating_reading_and_writing import *
print("Setup complete.")
```
### 1.In the cell below, create a DataFrame fruits that looks like this:
```python
fruits= mypd.DataFrame({'Apples':[30],'Bananas':[21]})
```
### 2.Create a dataframe fruit_sales that matches the diagram below:
```python
fruit_sales=mypd.DataFrame({'Apples':[35,41],'Bananas':[21,34]},index=['2017 Sales','2018 Sales'])
fruit_sales
```
### 3. Create a variable ingredients with a Series that looks like:
```python
ingredients =mypd.Series(['4 cups','1 cup','2 large','1 can'],index=['Flour','Milk','Eggs','Spam'],name='Dinner')
```

### 4. Read the following csv dataset of wine reviews into a DataFrame called reviews:
```python
reviews = mypd.read_csv("../input/wine-reviews/winemag-data_first150k.csv",index_col=0)
reviews
```

### 5. Run the cell below to create and display a DataFrame called animals:
```python
animals = mypd.DataFrame({'Cows': [12, 20], 'Goats': [22, 19]}, index=['Year 1', 'Year 2'])
print(animals)
# Your code goes here
animals.to_csv("cows_and_goats.csv")
```

## Indexing, Selecting & Assigning

- In Python, we can access the property of an object by accessing it as an attribute. A book object, for example, might have a title property, which we can access by calling book.title.
- Columns in a pandas DataFrame work in much the same way.
- Hence to access the country property of reviews we can use:
```python
reviews.country
```
- If we have a Python dictionary, we can access its values using the indexing ([]) operator. We can do the same with columns in a DataFrame:
```python
reviews['country']
```


- These are the two ways of selecting a specific Series out of a DataFrame. Neither of them is more or less syntactically valid than the other, but the indexing operator [] does have the advantage that it can handle column names with reserved characters in them (e.g. if we had a country providence column, reviews.country providence wouldn't work).
- Doesn't a pandas Series look kind of like a fancy dictionary? It pretty much is, so it's no surprise that, to drill down to a single specific value, we need only use the indexing operator [] once more:
```python
reviews['country'][0]
```

### Indexing in pandas¶

Indexing in pandas
-The indexing operator and attribute selection are nice because they work just like they do in the rest of the Python ecosystem. As a novice, this makes them easy to pick up and use. However, pandas has its own accessor operators, loc and iloc. For more advanced operations, these are the ones you're supposed to be using.

### Index-based selection
- Pandas indexing works in one of two paradigms. The first is index-based selection: selecting data based on its numerical position in the data. iloc follows this paradigm.
- To select the first row of data in a DataFrame, we may use the following:
- Both loc and iloc are row-first, column-second.
```python
reviews.iloc[0]
reviews.iloc[:, 0]

#to select the country column from just the first, second, and third row, we would do:
reviews.iloc[:3, 0]

#Or, to select just the second and third entries, we would do:
reviews.iloc[1:3, 0]

#It's also possible to pass a list:
reviews.iloc[[0, 1, 2], 0]

#here are the last five elements of the dataset.
reviews.iloc[-5:]


```
### Label-based selection¶
- The second paradigm for attribute selection is the one followed by the loc operator: label-based selection. In this paradigm, it's the data index value, not its position, which matters.
- For example, to get the first entry in reviews, we would now do the following:
```python
reviews.loc[0, 'country']
```

- iloc is conceptually simpler than loc because it ignores the dataset's indices. When we use iloc we treat the dataset like a big matrix (a list of lists), one that we have to index into by position. loc, by contrast, uses the information in the indices to do its work. 
- here's one operation that's much easier using loc:
```python
reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
```

### Choosing between loc and iloc
- When choosing or transitioning between loc and iloc, there is one "gotcha" worth keeping in mind, which is that the two methods use slightly different indexing schemes.
- iloc uses the Python stdlib indexing scheme, where the first element of the range is included and the last one excluded. So 0:10 will select entries 0,...,9. loc, meanwhile, indexes inclusively. So 0:10 will select entries 0,...,10.

### Manipulating the index
-Label-based selection derives its power from the labels in the index. Critically, the index we use is not immutable. We can manipulate the index in any way we see fit.
-The set_index() method can be used to do the job. Here is what happens when we set_index to the title field:
-reviews.set_index("title")

### Conditional selection
- So far we've been indexing various strides of data, using structural properties of the DataFrame itself. To do interesting things with the data, however, we often need to ask questions based on conditions.
- For example, suppose that we're interested specifically in better-than-average wines produced in Italy.
- We can start by checking if each wine is Italian or not:
```python
 reviews.country == 'Italy'

#We can use the ampersand (&) to bring the two questions together:
 reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]
```











