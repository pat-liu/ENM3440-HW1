# Smash Ultimate Analysis: Popular Playstyles

## Introduction

Super Smash Brothers Ultimate is a crossover fighting game series published by Nintendo. Two players select characters to use from a collection of over 80 playable characters and face each other. The game is unique in that the aim is to increase damage counters and knock opponents off the stage instead of depleting life bars.

In this project, we aim to study the most popular "playstyle" archetypes, across all characters from a dataset of matches (tournaments). The dataset in CSV form can be found from [ultimate game data](https://ultimategamedata.com/about).

## Approach

### Load in Data

We import the above CSV dataset of tournament data into Google Drive, and then mount the data like so into our Jupyter Notebook. We must manually grant access to our Google account.

```python
from google.colab import drive

drive.mount("/content/gdrive")
full_df = pd.read_csv("gdrive/My Drive/full_raw_data.csv")
```

### Schema Overview

We use the head command to display the first five rows of the table, along with the corresponding columns.

```python
full_df.head()  # Display a portion of uncleaned data
full_df.shape # Get number of rows, columns: (3663709, 14)
```

![Head](/assets/head.png)

As seen, our indexes consist of:

```python
Index(['Timestamp', 'Tournament ID', 'Tournament Name', 'Event ID',
       'Event Name', 'Set ID', 'Game Number', 'Stage Name', 'Winner ID',
       'Loser ID', 'Winner Name', 'Loser Name', 'Winner Character',
       'Loser Character'],
      dtype='object')
```

In our mini-project, we primarily care about the characters used in each match, which are 'Winner Character' and 'Loser Character'.

### Data Cleaning

We inspect the values of the character columns, and filter out invalid placeholders ('Jacqui Briggs' appears to be a player name instead of a character, and 'Random Character' is far too arbitrary), removing the rows like so.

```python
full_df = full_df.loc[(full_df['Winner Character'] != 'Jacqui Briggs') & (full_df['Winner Character'] != 'Random Character')
                      & (full_df['Loser Character'] != 'Jacqui Briggs') & (full_df['Loser Character'] != 'Random Character')]

# Drop all rows with null/invalid values (in Winner/Loser Character)
full_df.dropna(inplace=True)
full_df.shape # Gives us (3435727, 14)
```

Since the number of rows has been reduced, we have succesfully cleaned null / invalid rows.

### Character Analysis

#### All Characters

Let us confirm all the available playable characters below with a simple printing of all seen characters in our dataset.

```python
characters = set(full_df['Winner Character'].tolist() + full_df['Loser Character'].tolist())
print(len(characters))
characters
```

We get the following output:

```python
85
{'Banjo-Kazooie',
 'Bayonetta',
 'Bowser',
 'Bowser Jr.',
 'Byleth',
 'Captain Falcon',
 'Chrom',
 'Cloud',
 'Corrin',
 'Daisy',
 'Dark Pit',
 'Dark Samus',
 'Diddy Kong',
 'Donkey Kong',
 'Dr. Mario',
 'Duck Hunt',
 'Falco',
 'Fox',
 'Ganondorf',
 'Greninja',
 'Hero',
 'Ice Climbers',
 'Ike',
 'Incineroar',
 'Inkling',
 'Isabelle',
 'Jigglypuff',
 'Joker',
 'Kazuya',
 'Ken',
 'King Dedede',
 'King K. Rool',
 'Kirby',
 'Link',
 'Little Mac',
 'Lucario',
 'Lucas',
 'Lucina',
 'Luigi',
 'Mario',
 'Marth',
 'Mega Man',
 'Meta Knight',
 'Mewtwo',
 'Mii Brawler',
 'Mii Gunner',
 'Mii Swordfighter',
 'Min Min',
 'Mr. Game & Watch',
 'Ness',
 'Olimar',
 'Pac-Man',
 'Palutena',
 'Peach',
 'Pichu',
 'Pikachu',
 'Piranha Plant',
 'Pit',
 'Pokemon Trainer',
 'Pyra & Mythra',
 'R.O.B.',
 'Richter',
 'Ridley',
 'Robin',
 'Rosalina',
 'Roy',
 'Ryu',
 'Samus',
 'Sephiroth',
 'Sheik',
 'Shulk',
 'Simon Belmont',
 'Snake',
 'Sonic',
 'Steve',
 'Terry',
 'Toon Link',
 'Villager',
 'Wario',
 'Wii Fit Trainer',
 'Wolf',
 'Yoshi',
 'Young Link',
 'Zelda',
 'Zero Suit Samus'}
```

#### Top 20 Used Characters

We plot the top 20 used characters in preparation for ranking the top playstyles.

```python
player_a_character_df = full_df['Winner Character']
player_b_character_df = full_df['Loser Character']

characters_df = pd.concat([player_a_character_df, player_b_character_df])
character_value_counts = characters_df.value_counts()

character_counts_df = character_value_counts.rename_axis('Character').reset_index(name='Counts')
character_counts_df = character_counts_df.sort_values("Counts", ascending=False).head(20)
p = sns.catplot(data=character_counts_df, kind="bar", x="Character", y="Counts", height=5, aspect=3, order =character_counts_df.sort_values('Counts').Character)
p.set_xticklabels(rotation=90)
```
