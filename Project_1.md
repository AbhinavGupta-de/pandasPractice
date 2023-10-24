import pandas as pd

df = pd.read_csv('words.csv', index_col='Word')

df.head()

### Activities

##### How many elements does this dataframe have?

df.size/2

df.shape

##### What is the value of the word `microspectrophotometries`?

df.loc["microspectrophotometries"]

##### What is the highest possible value of a word?

df['Value'].max()

df.max()

##### Which of the following words have a Char Count of `7` and a Value of `87`?

df['Value'] == 87



##### What is the highest possible length of a word?

df['Char Count'].max()

##### What is the word with the value of `319`?

df.loc[df['Value'] == 319]

##### What is the most common value?

df.mode()

df['Value'].mode()

df.describe()

df['Value'].describe()

##### What is the shortest word with value `274`?

df.loc[df['Value'] == 274].sort_values(by='Char Count')

df.loc[df['Value'] == 274, 'Char Count'].min()

##### Create a column `Ratio` which represents the 'Value Ratio' of a word

df['Ratio'] = df['Value'] / df['Char Count'] 

df.head()

##### What is the maximum value of `Ratio`?

df['Ratio'].max()

##### What word is the one with the highest `Ratio`?

df.loc[df['Ratio'] == df['Ratio'].max()]

##### How many words have a `Ratio` of `10`?

df.loc[df['Ratio'] == 10]

df.loc[df['Ratio'] == 10].shape

##### What is the maximum `Value` of all the words with a `Ratio` of `10`?

df.loc[df['Ratio'] == 10].max()

##### Of those words with a `Value` of `260`, what is the lowest `Char Count` found?

df['Char Count'].loc[df['Value'] == 260].min()

##### Based on the previous task, what word is it?

df.query("Value == 260").sort_values(by="Char Count")
