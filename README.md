# **The-Walking-Dead-Advanced-EDA**

## **Introduction**
![TWD Logo](img/TWD_logo_Readme.png)
The Walking Dead is an American zombie horror television series produced by [**AMC**](https://en.wikipedia.org/wiki/AMC_Networks). The show is based on acclaimed comic book series created by Robert Kirkman (also known for Invencible, Outacst, Oblivion Song ...), which follows a group of survivors led by former sheriff's deputy **Rick Grimes** as they navigate the dangers of this new world, including not only the relentless threat of the undead but also conflicts with other survivors, internal power struggles, and the struggle to maintain their humanity in the face of unimaginable horror.  
  
The series premiered on **October 31, 2010** and ended it's run with the **11th** and **final** season on **November 20, 2022** ending series after **177 episodes**. During it's 11 seasons The Walking Dead gained massive and supportive fanbase that is still engaged in the world of The Walking Dead. Based on a huge success creators developed to this date (February, 2024) **six other spin-off shows** like Fear The Walking Dead, The Walking Dead Dead City or The Walking Dead The Ones Who Live.  
  
<p align = "center">
  <b>"Hey, you. Dumbass." -Glenn Rhee</b>
</p>

## Table of content
- 📖[Introduction](#introduction)
- ⚙️[Data source](#data-source)
  - [Episodes](#episodes)
  - [IMDB](#imdb)
- 🔍[Data Cleaning And Preparation](#data-cleaning-and-preparation)
  - [Importing Libraries](#importing-libraries)
  - [Preparing Dataset](#preparing-dataset)
  - [Observing And Cleaning](#observing-and-cleaning)
- 💡[Recommendations](#recommendations)
- 🔖[Used Sources](#used-sources)

## Data Source
For this analysis I used dataset [The Walking Dead Episodes](https://www.kaggle.com/datasets/bcruise/the-walking-dead-episodes/data) (CC0: Public Domain, dataset made available through [Bill Cruise](https://www.kaggle.com/bcruise)).  

This dataset has data about all 177 episodes. Dataset has total two **.csv** files. Both of these files has some common data in them but also  some differenies.

### Episodes
First file is **the_walking_dead_episodes.csv** this file has total **eight** columns.

| **Column #**   | **Column Name**       | **Description**          | **Column #**    | **Column Name**    | **Description**                 |
|----------------|-----------------------|--------------------------|-----------------|--------------------|---------------------------------|
| **0**          | season                | season                   | **4**           | directed_by        | director(s) of the episode      |
| **1**          | episode_num_in_season | episode number in season | **5**           | written_by         | writter(s) of the episode       |
| **2**          | episode_num_overall   | episode number in series | **6**           | original_air_date  | original air date               |
| **3**          | title                 | title of the episode     | **7**           | us_viewers         | US viewers on original air date |

### IMDB
Second files is **the_walking_dead_imdb.csv** this file has less columns than previous, just **seven**.

| **Column #**   | **Column Name**       | **Description**          | **Column #**    | **Column Name**    | **Description**                                             |
|----------------|-----------------------|--------------------------|-----------------|--------------------|-------------------------------------------------------------|
| **0**          | season                | season                   | **4**           | imdb_rating        | average IMDB rating                                         |
| **1**          | episode_num_in_season | episode number in season | **5**           | total_votes        | total number of votes that <br>the IMDB rating was based on |
| **2**          | title                 | title of the episode     | **6**           | desc               | episode synopsis                                            |
| **3**          | original_air_date     | original air date        |                 |                    |                                                             |

<p align = "center">
  <b>"We are the walking dead." -Rick Grimes</b>
</p>

## Data Cleaning And Preparation
Before I started with analysis in Python I took a peek at both files in Excel. Because both files has almost same structure and share majority of columns I diceded to merge these two files into before diving into analysis.<br>
  
### Importing Libraries
I start my analysis by importing libraries of Python packages for needs of my analysis.
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import ttest_ind
from scipy.stats import kruskal
```

### Preparing Dataset
Then I loaded both files into dataframes.
```python
episodes = pd.read_csv('the_walking_dead_episodes.csv')
imdb = pd.read_csv('the_walking_dead_imdb.csv')
```
With both files successfuly loaded I merged them into one dataset that I will be using for rest of the analysis.
```python
twd_dataset = pd.merge(episodes, imdb)
```
After I merged both files into one dataset I check created dataset if everything is like it should be.
```python
print(twd_dataset.shape)

Output:
(177,12)
```
```python
twd_dataset.head()

Output:
| season | episode_num_in_season | episode_num_overall | title                | directed_by           | written_by                                  | original_air_date | us_viewers  | episode_num | imdb_rating | total_votes | desc                                              |
|--------|-----------------------|---------------------|----------------------|-----------------------|---------------------------------------------|-------------------|-------------|-------------|-------------|-------------|---------------------------------------------------|
| 1      | 1                     | 1                   | Days Gone Bye        | Frank Darabont        | Teleplay by: Frank Darabont                 | 2010-10-31        | 5350000.0   | 1           | 9.2         | 27463       | Deputy Sheriff Rick Grimes awakens from a coma... |
| 1      | 2                     | 2                   | Guts                 | Michelle MacLaren     | Frank Darabont                              | 2010-11-07        | 4710000.0   | 2           | 8.6         | 17278       | In Atlanta, Rick is rescued by a group of surv... |
| 1      | 3                     | 3                   | Tell It to the Frogs | Gwyneth Horder-Payton | Story by: Charles H. Eglee & Jack LoGiudice | 2010-11-14        | 5070000.0   | 3           | 8.2         | 15815       | Rick is reunited with Lori and Carl but soon d... |
| 1      | 4                     | 4                   | Vatos                | Johan Renck           | Robert Kirkman                              | 2010-11-21        | 4750000.0   | 4           | 8.5         | 15449       | Rick, Glenn, Daryl and T-Dog come across a gro... |
| 1      | 5                     | 5                   | Wildfire             | Ernest Dickerson      | Glen Mazzara                                | 2010-11-28        | 5560000.0   | 5           | 8.1         | 14856       | After the attack on the camp, Rick leads the s... |

```
Dataset now has **177 rows** and **12 columns**. Number of rows is correct but when it comes to columns noticed that there was one column twice but with different naming. These two columns were `episode_num_in_season` and `episode_num`. Both of them had same meaning so I decide to drop one of them to keep my dataset clean.
```python
twd_dataset = twd_dataset.drop(['episode_num'], axis= 'columns')
```
I checked again shape of the dataset to make sure all changes were applied and column was removed.
```python
twd_dataset.shape

Output:
(177, 11)
```
```python
twd_dataset.head()

Output:
| season | episode_num_in_season | episode_num_overall | title                | directed_by           | written_by                                  | original_air_date | us_viewers  | imdb_rating | total_votes | desc                                              |
|--------|-----------------------|---------------------|----------------------|-----------------------|---------------------------------------------|-------------------|-------------|-------------|-------------|---------------------------------------------------|
| 1      | 1                     | 1                   | Days Gone Bye        | Frank Darabont        | Teleplay by: Frank Darabont                 | 2010-10-31        | 5350000.0   | 9.2         | 27463       | Deputy Sheriff Rick Grimes awakens from a coma... |
| 1      | 2                     | 2                   | Guts                 | Michelle MacLaren     | Frank Darabont                              | 2010-11-07        | 4710000.0   | 8.6         | 17278       | In Atlanta, Rick is rescued by a group of surv... |
| 1      | 3                     | 3                   | Tell It to the Frogs | Gwyneth Horder-Payton | Story by: Charles H. Eglee & Jack LoGiudice | 2010-11-14        | 5070000.0   | 8.2         | 15815       | Rick is reunited with Lori and Carl but soon d... |
| 1      | 4                     | 4                   | Vatos                | Johan Renck           | Robert Kirkman                              | 2010-11-21        | 4750000.0   | 8.5         | 15449       | Rick, Glenn, Daryl and T-Dog come across a gro... |
| 1      | 5                     | 5                   | Wildfire             | Ernest Dickerson      | Glen Mazzara                                | 2010-11-28        | 5560000.0   | 8.1         | 14856       | After the attack on the camp, Rick leads the s... |

```
Also I renamed column `desc` which has synopsis of episode to `episode_synopsis` for much clearer naming.
```python
twd_dataset.rename(columns = {'desc': 'episode_synopsis'}, inplace = True)
```
### Observing And Cleaning
At this moment dataset was ready so I took a step and looked at dataste more closely. First of all I check some basic informations about dataset.
```python
twd_dataset

Output:
| season | episode_num_in_season | episode_num_overall | title                | directed_by           | written_by                                           | original_air_date | us_viewers  | episode_num | imdb_rating | total_votes | episode_synopsis                                      |
|--------|-----------------------|---------------------|----------------------|-----------------------|------------------------------------------------------|-------------------|-------------|-------------|-------------|-------------|-------------------------------------------------------|
| 1      | 1                     | 1                   | Days Gone Bye        | Frank Darabont        | Teleplay by: Frank Darabont                          | 2010-10-31        | 5350000.0   | 1           | 9.2         | 27463       | Deputy Sheriff Rick Grimes awakens from a coma...     |
| 1      | 2                     | 2                   | Guts                 | Michelle MacLaren     | Frank Darabont                                       | 2010-11-07        | 4710000.0   | 2           | 8.6         | 17278       | In Atlanta, Rick is rescued by a group of surv...     |
| 1      | 3                     | 3                   | Tell It to the Frogs | Gwyneth Horder-Payton | Story by: Charles H. Eglee & Jack LoGiudiceTele...   | 2010-11-14        | 5070000.0   | 3           | 8.2         | 15815       | Rick is reunited with Lori and Carl but soon d...     |
| 1      | 4                     | 4                   | Vatos                | Johan Renck           | Robert Kirkman                                       | 2010-11-21        | 4750000.0   | 4           | 8.5         | 15449       | Rick, Glenn, Daryl and T-Dog come across a gro...     |
| 1      | 5                     | 5                   | Wildfire             | Ernest Dickerson      | Glen Mazzara                                         | 2010-11-28        | 5560000.0   | 5           | 8.1         | 14856       | After the attack on the camp, Rick leads the s...     |
| ...    | ...                   | ...                 | ...                  | ...                   | ...                                                  | ...               | ...         | ...         | ...         | ...         | ...                                                   |
| 11     | 20                    | 173                 | What's Been Lost     | Aisha Tyler           | Erik Mountain                                        | 2022-10-16        | 1360000.0   | 20          | 7.3         | 4474        | Daryl and Carol search for their disappeared f...     |
| 11     | 21                    | 174                 | Outpost 22           | Tawnia McKiernan      | Jim Barnes                                           | 2022-10-23        | 1500000.0   | 21          | 7.2         | 4363        | The survivors track a convoy to a mysterious d...     |
| 11     | 22                    | 175                 | Faith                | Rose Troche           | Nicole Mirante-Matthews & Magali Lozano              | 2022-10-30        | 1390000.0   | 22          | 7.9         | 4891        | Ezekiel and Negan plan a work camp revolt; Eug...     |
| 11     | 23                    | 176                 | Family               | Sharat Raju           | Magali Lozano & Erik Mountain & Kevin Deiboldt       | 2022-11-06        | 1470000.0   | 23          | 8.6         | 6483        | Reunited, the group heads back to the commonwe...     |
| 11     | 24                    | 177                 | Rest in Peace        | Greg Nicotero         | Story by: Angela KangTeleplay by: Corey Reed &...    | 2022-11-20        | 2270000.0   | 24          | 8.3         | 11220       | Our survivors must save their kids and the Com...     |

```
```python
twd_dataset.info()

Output:
<class 'pandas.core.frame.DataFrame'>
Int64Index: 177 entries, 0 to 176
Data columns (total 12 columns):
 #   Column                 Non-Null Count  Dtype  
---  ------                 --------------  -----  
 0   season                 177 non-null    int64  
 1   episode_num_in_season  177 non-null    int64  
 2   episode_num_overall    177 non-null    int64  
 3   title                  177 non-null    object 
 4   directed_by            177 non-null    object 
 5   written_by             177 non-null    object 
 6   original_air_date      177 non-null    object 
 7   us_viewers             177 non-null    float64
 8   episode_num            177 non-null    int64  
 9   imdb_rating            177 non-null    float64
 10  total_votes            177 non-null    int64  
 11  desc                   177 non-null    object 
dtypes: float64(2), int64(5), object(5)
memory usage: 22.0+ KB
```
**Key Takeaways:**  
- dataset has **177 rows** and **11 columns**
- **non** of the columns has **empty fields**
- `original_air_date` and `us_viewers` has **wrong data types**
```python
twd_dataset.duplicated().sum()

Output:
0
```
**Key Takeaway:**  
- dataset has **0** duplicated rows
```python
display(twd_dataset.describe())

Output:
|           | season     | episode_num_in_season | episode_num_overall | us_viewers   | imdb_rating | total_votes  |
|-----------|------------|-----------------------|---------------------|--------------|-------------|--------------|
| count     | 177.000000 | 177.000000            | 177.000000          | 1.770000e+02 | 177.000000  | 177.000000   |
| mean      | 6.711864   | 9.135593              | 89.000000           | 8.137966e+06 | 7.915819    | 11266.056497 |
| std       | 3.078799   | 5.490046              | 51.239633           | 4.581610e+06 | 0.816922    | 5123.780686  |
| min       | 1.000000   | 1.000000              | 1.000000            | 1.190000e+06 | 4.100000    | 4325.000000  |
| 25%       | 4.000000   | 5.000000              | 45.000000           | 3.660000e+06 | 7.400000    | 7653.000000  |
| 50%       | 7.000000   | 9.000000              | 89.000000           | 7.920000e+06 | 7.900000    | 11220.000000 |
| 75%       | 10.000000  | 13.000000             | 133.000000          | 1.238000e+07 | 8.500000    | 13039.000000 |
| max       | 11.000000  | 24.000000             | 177.000000          | 1.729000e+07 | 9.600000    | 42427.000000 |
```
**Key Takeaways:**  
- seasons are from **1** to **11**
- episdoes are from **1** to **177**
- average US viewers, IMDB rating and total votes will be **analyzed more in depth later**
```python
display(twd_dataset.describe(include = 'object'))

Output:
|               | title          | directed_by     | written_by   | episode_synopsis                                     |
|---------------|----------------|-----------------|--------------|------------------------------------------------------|
| count         | 177            | 177             | 177          | 177                                                  |
| unique        | 177            | 55              | 69           | 177                                                  |
| top           | Days Gone Bye  | Greg Nicotero   | Angela Kang  | Deputy Sheriff Rick Grimes awakens from a coma...    |
| freq          | 1              | 37              | 15           | 10                                                   |
```
**Key Takeaways:**  
**_Please Note:_**  
Later I discovered that names of some directors are not cleaned so take this part as initial look at data not final conclusion.
- **Greg Nicotero** directed most episodes
- **Angela Kang** wrote most episodes
```python
twd_dataset.isnull().apply(pd.Series.value_counts)

Output:
| season | episode_num_in_season | episode_num_overall | title | directed_by | written_by | original_air_date | us_viewers  | episode_num | imdb_rating | total_votes | episode_synopsis |
|--------|-----------------------|---------------------|-------|-------------|------------|-------------------|-------------|-------------|-------------|-------------|------------------|
| False  | 177                   | 177                 | 177   | 177         | 177        | 177               | 177         | 177         | 177         | 177         | 177              |
```
**Key Takeaway:**  
- **non** of the columns has missing values
```python
twd_dataset.isna().apply(pd.Series.value_counts)

Output:
| season | episode_num_in_season | episode_num_overall | title | directed_by | written_by | original_air_date | us_viewers  | imdb_rating | total_votes | episode_synopsis |
|--------|-----------------------|---------------------|-------|-------------|------------|-------------------|-------------|-------------|-------------|------------------|
| False  | 177                   | 177                 | 177   | 177         | 177        | 177               | 177         | 177         | 177         | 177              |
```
**Key Takeaway:**  
- **non** of the columns has NaN values
  
As I mentioned above columns `original_air_date` and `us_viewers` has a wrong data types. So I decide to chang `original_air_date` from `object`  to `datetime64[ns]` and `us_viewers` to `int`.
```python
twd_dataset['us_viewers'] = twd_dataset['us_viewers'].astype('int')
twd_dataset['original_air_date'] = pd.to_datetime(twd_dataset['original_air_date'])
```
```python
twd_dataset.dtypes

Output:
season                            int64
episode_num_in_season             int64
episode_num_overall               int64
title                            object
directed_by                      object
written_by                       object
original_air_date        datetime64[ns]
us_viewers                        int64
imdb_rating                     float64
total_votes                       int64
episode_synopsis                 object
dtype: object
```
Before I moved to some actual analysis lastly I looked at what values are stored in columns and also checked how many values are unique so I woudn't japerdise analysis by skipping this important step. I used **for** loop to get info about all colums.  
**_Please Note:_**  
I didn't include output of this code because it's too long, but I will provide my findings.
```python
cols = twd_dataset.columns.tolist()

for stat in cols:
  print(stat)
  print(twd_dataset[stat].unique())
  print('Number of unique: ' + str(twd_dataset[stat].nunique()))
  print('\n')
```
**Key Takeaways:**
- there are **11** unique seasons
- **24** unique episode numbers
- **177** unique overall episodes
- **177** unique titles
- **55** directors (with one badly formatted row)
- **69** writters (**9** that needs to be **corrected**)
- incosistent using of **"&"** and **"and"** in `directed_by` and `written_by ` column
- **176** unique dates I had to check it but it's correct because epsides **2x8** and **2x9** aired at **same day**
- **162** unique numbers of viewers
- **35** unique IDMB average ratings
- **176** different vote counts
- and as last **177** unique synopsis

So some things needed to be updated and changed to make this dataset ready to use. Firstly I fixed inconsistent usage of "&" and "and" at rows where episode was written by **more than one** writer.
```python
twd_dataset['written_by'].replace('&', 'and', inplace = True, regex = True)
```
Second thing that need to be fixed was one value in `diredted_by` where names were separated with **"space"** so I changed that to **"and"**.
```python
twd_dataset.loc[twd_dataset['directed_by'] == 'Ernest DickersonGwyneth Horder-Payton', 'directed_by'] = 'Ernest Dickerson and Gwyneth Horder-Payton'
```
Here I noticed that person named **"David Leslie Johnson"** sometimes appears as **"David Leslie Johnson-McGoldrick"** so I make sure to fix this along with all faulty "&" and "and" usage in `written_by` column.
```python
twd_dataset.loc[twd_dataset['written_by'] == 'David Leslie Johnson', 'written_by'] = 'David Leslie Johnson-McGoldrick'

twd_dataset.loc[twd_dataset['written_by'] == 'Teleplay by: Frank Darabont', 'written_by'] = 'Frank Darabont'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Angela KangTeleplay by: Corey Reed and Jim Barnes', 'written_by'] = 'Angela Kang and Corey Reed and Jim Barnes'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Charles H. Eglee and Jack LoGiudiceTeleplay by: Charles H. Eglee and Jack LoGiudice and Frank Darabont', 'written_by'] = 'Charles H. Eglee and Jack LoGiudice and Frank Darabont'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Corey ReedTeleplay by: Corey Reed and Kevin Deiboldt', 'written_by'] = 'Corey Reed and Kevin Deiboldt'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Jim Barnes and Eli Jorné and Corey ReedTeleplay by: Corey Reed', 'written_by'] = 'Jim Barnes and Eli Jorné and Corey Reed'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Scott M. Gimple and Channing PowellTeleplay by: Channing Powell', 'written_by'] = 'Scott M. Gimple and Channing Powell'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Scott M. Gimple and David Leslie Johnson-McGoldrick and Angela KangTeleplay by: David Leslie Johnson-McGoldrick and Angela Kang', 'written_by'] = 'Scott M. Gimple and David Leslie Johnson-McGoldrick and Angela Kang'
twd_dataset.loc[twd_dataset['written_by'] == 'Story by: Scott M. Gimple and Matthew NegreteTeleplay by: Matthew Negrete', 'written_by'] = 'Scott M. Gimple and Matthew Negrete'
```
After this step number of **directors** changed from **57** to **55** and number of **writers** chnged from **69** to **67**. Dataset was finally complete and clean so I saved it before moving on. 
```python
twd_dataset.to_csv('TWD_dataset.csv', encoding = 'utf-8', index = False)
```
At this point I did everything with data in phase of preparing data for analysis. So let's move on to some **REAL** analysis stuff.🦸
