# **The-Walking-Dead-Advanced-EDA**

## **Introduction**
The Walking Dead is an American zombie horror television series produced by AMC. The show is based on acclaimed comic book series created by Robert Kirkman (Invencible, Outacst, Oblivion Song ...), which follows a group of survivors led by former sheriff's deputy Rick Grimes as they navigate the dangers of this new world, including not only the relentless threat of the undead but also conflicts with other survivors, internal power struggles, and the struggle to maintain their humanity in the face of unimaginable horror.  
  
The series premiered on October 31, 2010 and ended its run with the 11th and final season on November 200, 2022 ending series after 177 episodes. During it's 11 seasons The Walking Dead gained massive and supportive fanbase that is still engaged in the world of The Walking Dead. Based on a huge success creators developed to this date (February, 2024) six other spin-off shows like Fear The Walking Dead, The Walking Dead Dead City or The Walking Dead The Ones Who Live.  

<p align = "center">
  <b>"Hey, you. Dumbass." -Glenn Rhee</b>
</p>

## Table of content
- [Introduction](#introduction)
- [Data source](#data-source)
  - [episodes file](#episodes)
  - [IMDB file](#imdb)
- [Analysis](#analysis)

## Data Source
For this analysis we used dataset [The Walking Dead Episodes](https://www.kaggle.com/datasets/bcruise/the-walking-dead-episodes/data) (CC0: Public Domain, dataset made available through [Bill Cruise](https://www.kaggle.com/bcruise)).  

This dataset has data about all 177 episodes. Dataset has total two **.csv** files. Both of these files has some common data in them but also  some differenies. We will take a peek at these files.  

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
