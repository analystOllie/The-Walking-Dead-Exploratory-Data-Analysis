# **The-Walking-Dead-Advanced-EDA**

## **Introduction**
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
- 🔍[Analysis](#analysis)
  - [Importing libraries](#importing-libraries)
  - [Preparing dataset](#preparing-dataset)
  - [Looking at the dataset](#looking-at-the-dataset)
- 💡[Recommendations](#recommendations)
- 🔖[Used sources](#used-sources)

## Data Source
For this analysis I used dataset [The Walking Dead Episodes](https://www.kaggle.com/datasets/bcruise/the-walking-dead-episodes/data) (CC0: Public Domain, dataset made available through [Bill Cruise](https://www.kaggle.com/bcruise)).  

This dataset has data about all 177 episodes. Dataset has total two **.csv** files. Both of these files has some common data in them but also  some differenies.

### Episodes
First file is **the_walking_dead_episodes.csv** this file has total **eight** columns.

| **Column #**   | **Column Name**   | **Description**   | **Column #**    | **Column Name**    | **Description**   |
|-------------|-------------|-------------|-------------|-------------|-------------|
| **0** | season | season | **4** | directed_by | director(s) of the episode |
| **1** | episode_num_in_season | episode number in season | **5** | written_by | writter(s) of the episode |
| **2** | episode_num_overall | episode number in series | **6** | original_air_date | original air date |
| **3** | title | title of the episode | **7** | us_viewers | US viewers on original air date |

### IMDB
Second files is **the_walking_dead_imdb.csv** this file has less columns than previous, just **seven**.

| **Column #**   | **Column Name**   | **Description**   | **Column #**    | **Column Name**    | **Description**   |
|-------------|-------------|-------------|-------------|-------------|-------------|
| **0** | season | season | **4** | imdb_rating | average IMDB rating |
| **1** | episode_num_in_season | episode number in season | **5** | total_votes | total number of votes that <br>the IMDB rating was based on |
| **2** | title | title of the episode | **6** | desc | episode synopsis |
| **3** | original_air_date | original air date |  |  |  |

## Analysis
Before I started with analysis in Python I took a peek at both files in Excel. Because both files has almost same structure and share majority of columns I diceded to merge these two files into before diving into analysis.  
  
### Importing libraries
I start my analysis by importing libraries of Python packages for needs of my analysis.
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import ttest_ind
from scipy.stats import kruskal
```

### Preparing dataset
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
```
```
Output:
(177,12)
```
```python
twd_dataset.head()
```
```
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
I checked again shape of the dataset to make sure all changes were applied.
```python
twd_dataset.shape
```
```
Output:
(177, 11)
```
Also I renamed column `desc` which has synopsis of episode to `episode_synopsis` for much clearer naming.
```python
twd_dataset.rename(columns = {'desc': 'episode_synopsis'}, inplace = True)
```
### Looking at the dataset
At this moment dataset was ready so I took a step and looked at dataste more closely. First of all I check some basic informations about dataset.
```python
```
