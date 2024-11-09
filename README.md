# Incentives

# Activity

## Overview of Dataset
_**Importing**_ these libraries is crucial to creating an output with clear visualization of data

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

To use and display the provided .csv file, we proceed with this code
```python
data = pd.read_csv('spotify-2023.csv')
data
```
However, this leads to a UnicodeDecodeError due to certain special characters.
```python
UnicodeDecodeError: 'utf-8' codec can't decode bytes in position 7250-7251: invalid continuation byte
```
To fix this problem we can add '*__encoding='latin-1'__*' in our code
```python
data = pd.read_csv('spotify-2023.csv', encoding='latin-1')
data
```
![image](https://github.com/user-attachments/assets/a6336aa8-1af8-4290-9c84-1db2c7a2019c)


To display the size of the whole Dataset we can use the .shape function
```python
size = data.shape
print(f"The Spotify Dataset contains {size[0]} rows & {size[1]} columns")
```
__OUTPUT:__
```python
The Spotify Dataset contains 953 rows & 24 columns
```
Moreover, the .info function can provide all the datatypes on each column of the Dataset
```python
print("\033[1m Data types of each column: \033[0m")
data.info()
```
__OUTPUT:__
```python
Data types of each column: 
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 953 entries, 0 to 952
Data columns (total 24 columns):
 #   Column                Non-Null Count  Dtype 
---  ------                --------------  ----- 
 0   track_name            953 non-null    object
 1   artist(s)_name        953 non-null    object
 2   artist_count          953 non-null    int64 
 3   released_year         953 non-null    int64 
 4   released_month        953 non-null    int64 
 5   released_day          953 non-null    int64 
 6   in_spotify_playlists  953 non-null    int64 
 7   in_spotify_charts     953 non-null    int64 
 8   streams               953 non-null    object
 9   in_apple_playlists    953 non-null    int64 
 10  in_apple_charts       953 non-null    int64 
 11  in_deezer_playlists   953 non-null    object
 12  in_deezer_charts      953 non-null    int64 
 13  in_shazam_charts      903 non-null    object
 14  bpm                   953 non-null    int64 
 15  key                   858 non-null    object
 16  mode                  953 non-null    object
 17  danceability_%        953 non-null    int64 
 18  valence_%             953 non-null    int64 
 19  energy_%              953 non-null    int64 
 20  acousticness_%        953 non-null    int64 
 21  instrumentalness_%    953 non-null    int64 
 22  liveness_%            953 non-null    int64 
 23  speechiness_%         953 non-null    int64 
dtypes: int64(17), object(7)
memory usage: 178.8+ KB
```

To check for any missing values in the Dataset we can use this set of code
```python
missing = data.isnull().sum()
print("\033[1m Missing values in each column: \033[0m")
print(missing)
total_missing = missing.sum()
print(f"The total missing values in each column: {total_missing}")
```
__OUTPUT:__
```python
Missing values in each column: 
track_name               0
artist(s)_name           0
artist_count             0
released_year            0
released_month           0
released_day             0
in_spotify_playlists     0
in_spotify_charts        0
streams                  0
in_apple_playlists       0
in_apple_charts          0
in_deezer_playlists      0
in_deezer_charts         0
in_shazam_charts        50
bpm                      0
key                     95
mode                     0
danceability_%           0
valence_%                0
energy_%                 0
acousticness_%           0
instrumentalness_%       0
liveness_%               0
speechiness_%            0
dtype: int64
The total missing values in each column: 145
```


#### Summary

According to the displayed values, there are a total datatypes of 17 int and 7 objects 

*__int (17):__* <br> artist_count, released_year released_month released_day, in_spotify_playlists, in_spotify_charts, in_apple_playlisys, in_apple_charts, in_deezer_charts, bpm, danceability_%, valence_%, energy_%, acousticness_%, instrumentalness_%, liveness_%, and speechiness_%

*__object (7):__* <br> track_name, artist(s)_name, streams, in_deezer_playlists, in_shazam_charts, key, and mode 

There are also a total of __145 missing values__ from _in_shazam_charts_ with 50 and _key_ with 95

## Basic Descriptive Statistics
To calculate for the Mean, Median, and Standard Deviation (std) we can use the following functions built in python. <br>
__.mean()__ - For the Mean <br>
__.median()__ - For the Median <br>
__.std()__ - For the Standard Deviation <br>

```python
mean = data['streams'].mean()
median = data['streams'].median()
std = data['streams'].std()

print(f"Mean of streams: {mean}")
print(f"Median of streams: {median}")
print(f"Standard Deviation of streams: {std}")
```

However, this block of code results to an error because of certain non-numeric values inside the streams column thus the mean, median, and std functions are unable to operate. <br>

This can easily be resolved by using pandas function *__pd.to_numeric()__* to convert the non-numeric values inside the column. Moreover, we will include *__errors='coerce'__* by the end of code to convert non-numeric values to *__NaN__*

This what our code should look by now.
```python
mean = pd.to_numeric(data['streams'], errors='coerce').mean()
median = pd.to_numeric(data['streams'], errors='coerce').median()
std = pd.to_numeric(data['streams'], errors='coerce').std()

print(f"Mean of streams: {mean}")
print(f"Median of streams: {median}")
print(f"Standard Deviation of streams: {std}")
```
__OUTPUT:__
```python
Mean of streams: 514137424.93907565
Median of streams: 290530915.0
Standard Deviation of streams: 566856949.0388832
```
To display the distribution of tracks released each year and the distribution of artist by count, we will utilize the functions inside __matplotlib__ to create a bar graph for better visualization

```python
Distribution of Tracks Released each year
plt.style.use('fivethirtyeight')
data_filtered = data[data['released_year'] >= 1990]
data_filtered['released_year'].value_counts().sort_index().plot(kind='bar', figsize=(10, 6), color='lightpink')
plt.title('Distribution of Tracks Released by Year (Starting from 1991)', fontsize=14, fontweight='bold', color='hotpink')
plt.xlabel('Year', fontweight='bold', color='lightpink', fontsize=12)
plt.ylabel('Tracks Released', fontweight='bold', color='lightpink', fontsize=12)
plt.xticks(rotation=90, fontsize=12, fontweight='bold', color = 'hotpink', )
plt.yticks(fontsize=12, fontweight='bold', color = 'hotpink')
plt.show()

Distribution of Artist by count
plt.style.use('seaborn-v0_8-pastel')
data['artist_count'].value_counts().sort_index().plot(kind='bar', figsize=(10, 6), color='lightpink', width=0.8)
plt.title('Distribution of Tracks by Artist Count', fontsize=16, fontweight='bold', color='hotpink')
plt.xlabel('Number of Artists', fontsize=14, fontweight='bold', color='lightpink')
plt.ylabel('Number of Tracks', fontsize=14, fontweight='bold', color='lightpink')
plt.xticks(rotation=0, fontsize=12, fontweight='bold', color='hotpink')
plt.yticks(fontsize=12, fontweight='bold', color='hotpink')
plt.show()
```
![image](https://github.com/user-attachments/assets/ed077c99-b3a6-401c-9a72-53a78e15e06d)

![image](https://github.com/user-attachments/assets/4bce466b-dc25-4ccb-9936-524bcaf5cc71)

#### Summary

Basing on the visualization from the graphs, it is very clear that ever since 2010 there has been a steady incline of released track per year with 2022 being the most released tracks in a year.

Moreover, for the tracks released by an artist, it is visible from the graph that the most released track are from solo artists.

## Top Performers

To display the top 5 most streamed tracks in the Dataset and to indentify which is the most streamed of the all we can use this block of code

We first remove all the commas and convert all the values inside the column 'streams', then proceed to sort the column to find the top 5 most streame tracks.

```python
data['streams'] = pd.to_numeric(data['streams'].astype(str).str.replace(',', ''), errors='coerce')
streams_df = data.nlargest(5, 'streams').reset_index(drop=True)
streams_df
```
![image](https://github.com/user-attachments/assets/07df5e33-d511-4d4c-810f-8d04d7d22ebb)
To indentify the top 5 most frequent artist based in the number of tracks in the Dataset, we can use this code to breakdown and split the values inside the column to count each artist separately. Without this, a inaccurate count of artist will be made due to collaborations of artists and such

```python
artists = data['artist(s)_name'].str.split(', ').explode().value_counts().head(5)
artists_df = top_artists.reset_index()
artists_df.columns = ['Artist', 'Track Count']
artists_df.set_index('Artist')
artists_df
```
![image](https://github.com/user-attachments/assets/bf426486-16f7-4b09-8dc6-58626f34c241)
## Temporal Trends
To analyze the trend in the graph, we can utilize __matplotlib__ once again with this block of code to see the Distribution of Tracks released by year

```python
tracks_per_year = data['released_year'].value_counts().sort_index()
plt.figure(figsize=(20, 6))
tracks_per_year.plot(kind='area', color='lightpink')
plt.title('Distribution of Tracks by Released Year', fontsize=20, fontweight='bold', color='hotpink')
plt.xlabel('Year', fontsize=14, fontweight='bold', color='lightpink')
plt.ylabel('Number of Tracks', fontsize=14, fontweight='bold', color='lightpink')
plt.xticks(fontsize=14, fontweight='bold', color='hotpink')
plt.yticks(fontsize=14, fontweight='bold', color='hotpink')
plt.show()
```
![image](https://github.com/user-attachments/assets/8ef81bb5-ef55-4970-a555-254fa33ae2c8)

This shows that there has been a skyrocket in releasing of tracks during the 2020s

Moreover, we also graphed the tracks released per month to see any noticable patterns in the column.

```python
month = data['released_month'].value_counts().sort_index()
plt.figure(figsize=(15, 7))
plt.bar(month.index, month.values, color='lightpink')
plt.title('Number of Tracks Released per Month', fontsize=16, fontweight='bold', color='hotpink')
plt.xlabel('Month', fontsize=14, fontweight='bold', color='lightpink')
plt.ylabel('Number of Tracks', fontsize=14, fontweight='bold', color='lightpink')
plt.xticks(ticks=range(1, 13), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],fontsize=16, fontweight='bold', color='hotpink')
plt.yticks(fontsize=16, fontweight='bold',color='hotpink')
plt.show()
```
![image](https://github.com/user-attachments/assets/a757949f-70ac-4db3-a278-91e4f3e527ed)

During the months of January and May has the tracks released, this might be because of the effects of a newer year for January and the early start of summer for May.

## Genre and Music Characteristics

To find the correlation between streams and musical attributes we first have to graph it for visualization. In here we utilized seaborn's heatmap instead of matplotlib for a change.

```python
clean = data[['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%']].dropna()
correlation_matrix = clean.corr()
plt.figure(figsize=(20, 8))
sns.heatmap(correlation_matrix, annot=True, cmap="RdPu", fmt='.2f', linewidths=1, linecolor='black', cbar_kws={'shrink': 0.8}, annot_kws={'size': 15, 'weight': 'bold'}, vmin=-1, vmax=1)
plt.title('Correlation Matrix Between Streams and Musical Attributes', fontsize=18, fontweight='bold')
plt.tight_layout()
plt.show()
```
![image](https://github.com/user-attachments/assets/991038dc-004d-4d45-aa12-9192980e1b5d)

### Summary
According to the heatmap above, there are no attributes that has a positive correlation with stream for it is either negative or zero.

For danceability and energy, both showed a __0.20__ value with each other showig a positive correlation. This shows that when one increases to 0.20 the other one also increases to 0.20

For valance and acousticness, both showed a __-0.08__ value with each other showing a negative correlation. This also shows that when valance decreases to -0.08, the same thing also happens to acousticness.

## Platform Popularity
To see the platform that favors the most popular tracks, we can create a simple bar graph for visualization.

In this block of code, we first converted all the dataypes inside the playlists into integers to add them. We then proceed to use __matplotlib__ again to plot a bar graph for the data

```python
data['in_spotify_playlists'] = pd.to_numeric(data['in_spotify_playlists'], errors='coerce').fillna(0)
data['in_deezer_playlists'] = pd.to_numeric(data['in_deezer_playlists'], errors='coerce').fillna(0)
data['in_apple_playlists'] = pd.to_numeric(data['in_apple_playlists'], errors='coerce').fillna(0)

spotify_count = data['in_spotify_playlists'].sum()
deezer_count = data['in_deezer_playlists'].sum()
apple_count = data['in_apple_playlists'].sum()

platforms = ['Spotify', 'Deezer', 'Apple']
track_counts = [spotify_count, deezer_count, apple_count]

plt.figure(figsize=(20, 5))
plt.bar(platforms, track_counts, color='lightpink', width=0.2)
plt.title("Track Counts of Spotifty, Deezer, and Apple Playlists", color='hotpink', fontweight='bold')
plt.xlabel("Platforms", color='lightpink')
plt.ylabel("Total Track Counts", color='lightpink')
plt.xticks(fontsize=14, fontweight='bold', color='hotpink')
plt.yticks(fontsize=14, fontweight='bold', color='hotpink')
plt.show()
```
![image](https://github.com/user-attachments/assets/fb66aed3-a248-474b-af65-faad50ea1911)

### Summary
As can be seen in the graph above, the platform that favors the most popular tracks is __Spotify__. This is most probably because of Spotify's catering of other services such as audiobooks and podcasts rather than just music all in one app.


