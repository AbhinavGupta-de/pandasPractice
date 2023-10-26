import itertools
import pandas as pd

# The new library!
from thefuzz import fuzz, process

df1 = pd.read_csv('companies_1.csv')
df2 = pd.read_csv('companies_2.csv')

### Data Preprocessing

##### 1. Create the `df` dataframe containing the product of the two CSVs

df1.head()

df2.head()

df = pd.DataFrame(
    itertools.product(df1['CLIENT'].values, df2['Firm Name']),
    columns=['CSV 1', 'CSV 2']
)

### Calculating the Levenshtein distance

Now, we will learn how to calculate the Levenshtein distance between two strings. Here we will user `partial_ratio` function from the `fuzz` module to compute the "ratio" between two strings. The result is a number between `0` and `100`, with `100` indicating a "perfect" match. Please note that `partial_ratio` gives ratio of the shortest string length to the longest string length. For example, if the first string is `ABC` and the second string is `ABCD`, then the ratio will be `3/4 = 0.75`. 

fuzz.partial_ratio("Apple", "Apple Inc.")

fuzz.partial_ratio("Microsoft", "Apple Inc.")

fuzz.partial_ratio("Microsoft", "MSFT")

If we have list of strings, we can calculate the Levenshtein distance between each pair of strings in the list.

A = ["Apple", "Alphabet", "Microsoft"]
B = ["MSFT", "Alphabet/Google", "Apple inc."]

Below, we combined the two list `A` and `B` into a list of tuples `companies` using `product` function from `itertools` module. 

Then, we calculated the partial ratio for each pair of strings in the list `companies` using `partial_ratio` function from `fuzz`.

companies = list(itertools.product(A, B))
companies

for c1, c2 in companies:
    ratio = fuzz.partial_ratio(c1, c2)
    print(f"{c1} > {c2}: {ratio}")

You will see the greater the ratio, the more similar the strings are.

##### 2. Create a new column `Ratio Score` that contains the distance for all the rows in `df`

score = [fuzz.partial_ratio(c1, c2) for c1, c2 in df.values]

score[:10]

df['Ratio Score'] = score

##### 3. How many rows have a Ratio score of `90` or more?

df.loc[df['Ratio Score'] >= 90].shape

##### 4. What's the corresponding company in CSV2 to `AECOM` in CSV1?

df.loc[
    (
    (df['CSV 1'] == 'AECOM') & 
        (df['Ratio Score'] >= 90)
    )
]

##### 5. What's the corresponding CSV2 company of *Starbucks*?

df.loc[
    (
    (df['CSV 1'] == 'Starbucks') & 
        (df['Ratio Score'] >= 90)
    )
]

##### 6. Is there a matching company for `Pinnacle West Capital Corporation`?

df.loc[
    (
    (df['CSV 1'] == 'Pinnacle West Capital Corporation') & 
        (df['Ratio Score'] >= 90)
    )
]

##### 7. How many matching companies are there for `County of Los Angeles Deferred Compensation Program`?

df.loc[
    (
    (df['CSV 1'] == 'County of Los Angeles Deferred Compensation Program') & 
        (df['Ratio Score'] >= 90)
    )
]

##### 8. Is there a matching company for `The Queens Health Systems`?

df.loc[
    (
    (df['CSV 1'] == 'The Queens Health Systems') & 
        (df['Ratio Score'] >= 90)
    )
]

### The End!
