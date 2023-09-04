# Smash Ultimate Analysis: Popular Playstyles

## Introduction

Super Smash Brothers Ultimate is a crossover fighting game series published by Nintendo. Two players select characters to use from a collection of over 80 playable characters and face each other. The game is unique in that the aim is to increase damage counters and knock opponents off the stage instead of depleting life bars.

In this project, we aim to study the most popular "playstyle" archetypes, across all characters from a dataset of matches (tournaments). The dataset in CSV form can be found from [ultimate game data](https://ultimategamedata.com/about).

## Approach

### Load in Data

We import the above CSV dataset of tournament data into Google Drive, and then mount the data like so into our Jupyter Notebook.

```
from google.colab import drive

drive.mount("/content/gdrive")
full_df = pd.read_csv("gdrive/My Drive/full_raw_data.csv")
```
