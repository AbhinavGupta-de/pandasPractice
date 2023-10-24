## Project: Querying and Filtering Pokemon data

This project will help you practice your pandas querying and filtering skills. Let's begin!

<center>
<img src="./mikel-DypO_XgAE4Y-unsplash.jpg" >
    <p align="center">
        Photo by <a href="https://unsplash.com/@mykelgran?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Mikel</a> on <a href="https://unsplash.com/s/photos/pokemon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>.
    </p>
</center>  

### Task 0 - Setup

There isn't much to do here, we'll provide the required imports and the read the pokemon CSV we'll be working with.

import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("pokemon.csv")

df.head()

df.info()

df.describe()

#### Distribution of Pokemon Types:

df['Type 1'].value_counts().plot(kind='pie', autopct='%1.1f%%', cmap='tab20c', figsize=(10, 8))

#### Distribution of Pokemon Totals:

df['Total'].plot(kind='hist', figsize=(10, 8))

df['Total'].plot(kind='box', vert=False, figsize=(10, 5))

#### Distribution of Legendary Pokemons:

df['Legendary'].value_counts().plot(kind='pie', autopct='%1.1f%%', cmap='Set3', figsize=(10, 8))

### Basic filtering

Let's start with a few simple activities regarding filtering.

##### 1. How many Pokemons exist with an `Attack` value greater than 150?

Doing a little bit of visual exploration, we can have a sense of the most "powerful" pokemons (defined by their "Attack" feature). A boxplot is a great way to visualize this:

sns.boxplot(data=df, x='Attack')

df.loc[df['Attack'] > 150]

##### 2. Select all pokemons with a Speed of `10` or less

sns.boxplot(data=df, x='Speed')

slow_pokemons_df = df.loc[df['Speed'] <= 10]

##### 3. How many Pokemons have a `Sp. Def` value of 25 or less?

df.head()

df.loc[df["Sp. Def"] <= 25]

df.loc[df["Sp. Def"] <= 25].shape

##### 4. Select all the Legendary pokemons

# Try your code here
legendary_df = df.loc[df['Legendary']]

##### 5. Find the outlier

Find the pokemon that is clearly an outlier in terms of Attack / Defense:

ax = sns.scatterplot(data=df, x="Defense", y="Attack")
ax.annotate(
    "Who's this guy?", xy=(228, 10), xytext=(150, 10), color='red',
    arrowprops=dict(arrowstyle="->", color='red')
)

df.query("Attack == 10 and Defense == 230")

df.sort_values(by=["Defense", "Attack"], ascending=[False, True]).head(1)

### Advanced selection

Now let's use boolean operators to create more advanced expressions

##### 6. How many Fire-Flying Pokemons are there?

df.head()

(df['Type 1'] == 'Fire').sum()

(df['Type 2'] == 'Flying').sum()

(
    (df['Type 1'] == 'Fire') &
    (df['Type 2'] == 'Flying')
).sum()

df.loc[(
    (df['Type 1'] == 'Fire') &
    (df['Type 2'] == 'Flying')
)]

##### 7. How many 'Poison' pokemons are across both types?

(
    (df["Type 1"] == 'Poison') |
    (df["Type 2"] == 'Poison')
).sum()

##### 8. What pokemon of `Type 1` *Ice* has the strongest defense?

df.loc[df["Type 1"] == 'Ice'].sort_values(by="Defense", ascending=False).head()

##### 9. What's the most common type of Legendary Pokemons?

df.loc[df['Legendary'], 'Type 1'].value_counts()

##### 10. What's the most powerful pokemon from the first 3 generations, of type water?

df.loc[((df['Type 1'] == "Water") & (df['Generation'] < 4)) ].sort_values(by='Total', ascending=False)

##### 11. What's the most powerful Dragon from the last two generations?

df['Generation'].value_counts()

df.loc[
    
        ((df['Type 1'] == 'Dragon') |
        (df['Type 2'] == 'Dragon')) &
        (df['Generation'].isin({5, 6}))
    
].sort_values(by='Total', ascending=False)

##### 12. Select most powerful Fire-type pokemons

powerful_fire_df = df.loc[(df['Type 1'] == 'Fire') & (df['Attack'] > 100)]

##### 13. Select all Water-type, Flying-type pokemons

water_flying_df = powerful_fire_df = df.loc[(df['Type 1'] == 'Water') & (df['Type 2'] == 'Flying')]

##### 14. Select specific columns of Legendary pokemons of type Fire

legendary_fire_df = df.loc[((df['Type 1'] == 'Fire') & (df['Legendary'])), ["Name", "Attack", "Generation"]]

##### 15. Select Slow and Fast pokemons

This is the distribution of speed of the pokemons. The red lines indicate those bottom 5% and top 5% pokemons by speed:

ax = df['Speed'].plot(kind='hist', figsize=(10, 5), bins=100)
ax.axvline(df['Speed'].quantile(.05), color='red')
ax.axvline(df['Speed'].quantile(.95), color='red')

bottom_5 = df['Speed'].quantile(0.05)
top_5 = df['Speed'].quantile(.95)
(bottom_5, top_5)

df.loc[(df['Speed'] > 110) | (df['Speed'] < 25)]

slow_fast_df = df.loc[(df['Speed'] > 110) | (df['Speed'] < 25)]

##### 16. Find the Ultra Powerful Legendary Pokemon

fig, ax = plt.subplots(figsize=(14, 7))
sns.scatterplot(data=df, x="Defense", y="Attack", hue='Legendary', ax=ax)
ax.annotate(
    "Who's this guy?", xy=(140, 150), xytext=(160, 150), color='red',
    arrowprops=dict(arrowstyle="->", color='red')
)

df.loc[
    (df['Legendary']) &
    (df['Defense'] < 150) &
    (df['Attack'] < 160)
].sort_values(by=["Defense", "Attack"], ascending=[False, False])

### The End!
