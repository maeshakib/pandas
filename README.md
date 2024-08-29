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
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]
```
- Pandas comes with a few built-in conditional selectors, two of which we will highlight here.
- The first is isin. isin is lets you select data whose value "is in" a list of values. For example, here's how we can use it to select wines only from Italy or France:
```python
reviews.loc[reviews.country.isin(['Italy', 'France'])]
#The second is isnull (and its companion notnull). These methods let you highlight values which are (or are not) empty (NaN).
reviews.loc[reviews.price.notnull()]
```
### Assigning data
Going the other way, assigning data to a DataFrame is easy. You can assign either a constant value:

reviews['critic'] = 'everyone'


## Exercise pandas: Indexing, Selecting & Assigning

#### Select the description column from reviews and assign the result to the variable desc.
```python 
# Your code here
desc = reviews.description
desc
```
### 2. Select the first value from the description column of reviews, assigning it to variable first_description.
```python 
first_description = desc[0]
 ```
###  3. Select the first row of data (the first record) from reviews, assigning it to the variable first_row.
```python 
first_row = reviews.iloc[0]
first_row
 ```

### 4. Select the first 10 values from the description column in reviews, assigning the result to variable first_descriptions.
```python 
first_descriptions = reviews.description.iloc[:10]
first_descriptions
 ```

### 5. Select the records with index labels 1, 2, 3, 5, and 8, assigning the result to the variable sample_reviews.
- In other words, generate the following DataFrame:
```python 
sample_reviews = reviews.iloc[[1,2,3,5,8]]
sample_reviews
 ```
### 6. Create a variable df containing the country, province, region_1, and region_2 columns of the records with the index labels 0, 1, 10, and 100. In other words, generate the following DataFrame:
```python 
df = reviews.loc[[0,1,10,100],["country","province","region_1","region_2"]]
df
 ```

### 7. Create a variable df containing the country and variety columns of the first 100 records.
```python 
df = reviews.loc[0:99, ["country", "variety"]]
df
 ```

### 8. Create a DataFrame italian_wines containing reviews of wines made in Italy. Hint: reviews.country equals what?
```python
italian_wines = reviews[reviews['country'] == 'Italy']
 ```
### 9. Create a DataFrame top_oceania_wines containing all reviews with at least 95 points (out of 100) for wines from Australia or New Zealand.
```python
top_oceania_wines = reviews.loc[reviews.country.isin(['Australia', 'New Zealand']) & (reviews.points >= 95)]
 ```



## 03-summary-functions-and-maps Exercises
### 1.what is the median of the points column in the reviews DataFrame?
```python
median_points = reviews.points.median()
median_points
```

### 2.What countries are represented in the dataset? (Your answer should not include any duplicates.)


```python
countries = reviews.country.unique()
countries
```
3.
How often does each country appear in the dataset? Create a Series reviews_per_country mapping countries to the count of reviews of wines from that country.
```python
median_points = reviews.points.median()
median_points
```
4.
Create variable centered_price containing a version of the price column with the mean price subtracted.

(Note: this 'centering' transformation is a common preprocessing step before applying various machine learning algorithms.)
```python
centered_price = reviews.price - reviews.price.mean()
centered_price
```
5.
I'm an economical wine buyer. Which wine is the "best bargain"? Create a variable bargain_wine with the title of the wine with the highest points-to-price ratio in the dataset.
```python
bargain_wine = reviews.loc[(reviews.points / reviews.price).idxmax(), 'title']
bargain_wine
```
6.
There are only so many words you can use when describing a bottle of wine. Is a wine more likely to be "tropical" or "fruity"? Create a Series descriptor_counts counting how many times each of these two words appears in the description column in the dataset.
```python
tropical = reviews.description.map(lambda x: "tropical" in x).sum()
fruity = reviews.description.map(lambda x: "fruity" in x).sum()
descriptor_counts = pd.Series([tropical, fruity], index=['tropical', 'fruity'])

descriptor_counts
`7.
We'd like to host these wine reviews on our website, but a rating system ranging from 80 to 100 points is too hard to understand - we'd like to translate them into simple star ratings. A score of 95 or higher counts as 3 stars, a score of at least 85 but less than 95 is 2 stars. Any other score is 1 star.

Also, the Canadian Vintners Association bought a lot of ads on the site, so any wines from Canada should automatically get 3 stars, regardless of points.

Create a series star_ratings with the number of stars corresponding to each review in the dataset.``

```python
def rating(row):
    return 3 if row.points >= 95 or row.country == 'Canada' else (2 if row.points >= 85 else 1)
star_ratings = reviews.apply(rating, axis='columns')
star_ratings
```

```python
median_points = reviews.points.median()
median_points
```

```python
median_points = reviews.points.median()
median_points
```
