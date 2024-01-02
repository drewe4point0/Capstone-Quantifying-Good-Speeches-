# Capstone: Quantifying Successful Speeches

**Table of Contents:**
- [Introduction, Project Overview](#introduction-project-overview)
- [Problem Area, Those Affected, and Impact](#the-problem-area-and-those-affected)
- [Methodology](#methodology)
- [Data](#data)
- [Notebooks](#notebooks)
- [Configuration](#configuration)
- [Steps to Complete](#steps-to-complete)




## Introduction, Project Overview

What are the qualities that make a speech impactful?  

This project is to determine insight into what attributes of TED speeches correlate with their success.  It is our hope that insights will be gained for anyone who wishes to impact an audience using speeches, from business leaders giving company town-halls, to students defending their thesis, to keynote speakers looking to "wow" an audience.

This project is also an exercise by it's author (Drewe MacIver) to build predictive models.  The latter parts of this project will focus on building a predictive model to see how determinate the metrics we've identified actually are at predicting the success of a speech.  

#### Goal:

The goal of this project is to:
1. Determine the attributes of speeches that correlate with that speeches success ("Correlation Analysis" / "CA").
2. Create a prediction model that can predict the rating of a speech ("Prediction Model" / "PM").




## The Problem Area and Those Affected

Effective communication is about influencing the audience.  A speech is an opportunity motivate and inspire.  To influence the mind of the listener and convey ideas that can lead to a change in their thoughts and a change in their actions.

There are several reasons why a speech is a particularly influential medium of communication:
- Duration: A speech is long enough to be able to use emphatic examples and oratory to accurately and entertainingly describe specific situations and things. 
- Curated / Specific Audience: The audience of a speech often shares common attributes (knowledge and values).  These similaraties can be capitalized on by speaking to the shared knowledge or values of the audience. 
- Specific Language: As speeches can be rehearsed and refined, precise language can be used to achieve a specific emotional result.  
- Oration / Dramatization: The speech giver is the sole provider of stimulus.  Their tone and pauses are given maximum attention and, therefore, maximum potency.  
- Historical Pre-disposition: The speech, in story form, has been the mode of communication between humans for thousands of years.  There may be a primal receptivity to this mode of communication.
- Attention (for in-person speeches): Unlike most mediums of communication, the attention of the in-person audience is somewhat mandatory.  Butts will stay in the seats and their choice to 'leave' is limited.  This allows for less sensationalist attention-holding fluff, and provides an opportunity for more thorough communication.

Every speech is an opportunity to influence.  Research into the attributes that cause a speech to have have an impact can benefit **anyone** who wishes to influence an audience. 




## Methodology:

Using a collection of 4,003 speeches collected from the TED website [What Is TED?](#data), this project will take the following steps:

1. Exploration of the data to determine a metric that we will use to determine what "good" means.
2. Further exploratory data analysis ("EDA") to determine which attributes can be quantified and used in our Correlation Analysis ("CA") or Prediction Model ("PM").
3. Correlation Analysis: An initial report on the measures that most strongly correlate with what a "good" speech is.
4. Prediction Model: Model building and refining to predict the "goodness" of a speech.




## Data: 

There are two main datasets that are used in the project.  Both were sourced from TED.com, using the [TED Scraper](https://github.com/corralm/ted-scraper/blob/main/README.md), and posted to [Kaggle.com](https://www.kaggle.com/) by individuals that are not connected to this project.  

The first dataset used (sourced from Kaggle.com) can be found [here](https://www.kaggle.com/datasets/miguelcorraljr/ted-ultimate-dataset/data). It's data dictionary can be found below.

The second dataset used (sourced from Kaggle.com) can be found [here](https://www.kaggle.com/datasets/miguelcorraljr/ted-talks-2022). It's data dictionary can be found below.

Special thanks to [Miguel Corral Jr.](https://www.kaggle.com/miguelcorraljr) for compiling both datasets used in this project.  


### Data Dictionaries:

#### Dataset #1:

This dataset is used because it contains the transcript for each talk given.  It was last updated on April 30th, 2020, and therefore contains fewer talks than Dataset #2 which was updated on October 13th, 2022.

The column breakdown of the dataset (n=4005):

- **talk_id:** Unique id for each individual talk (speech). (4005 unique values)
- **title:**  Title of the talk. (4005 unique values)
- **speaker_1:**  The speaker giving the talk. (3274 unique values)
- **all_speakers:** If there are multiple speakers, they will be listed here. (3307 unique values)
- **occupations:** The occupations of the speaker(s). (2050 unique values)
- **about_speakers:** Details about the speaker's background. (2978 unique values)
- **views:**  The number of video views that talk has on the TED website (as of April 30th, 2020) (3996 unique values)
- **recorded_date:** The date the talk was recorded. (1335 unique values)
- **published_date:** The date the talk was published on the TED.com website. (2962 unique values)
- **event:**  The TED event that the talk was given at.  (459 unique values)
- **native_lang:**  The language in which the TED talk was given.  (12 unique values)
- **available_lang:**  The available languages that the TED talk is available in.  (3902 unique values)
- **comments:**  The number of comments left on the TED website for that talk.  (602 unique values)
- **duration:**  The length (in seconds) of the talk. (1188 unique values)
- **topics:**  The topics that the TED talk covers.  (3977 unique values)
- **related_talks:**  The talk_id and title of other related TED talks.  (4005 unique values)
- **url:**  The URL of the TED talk.  (4005 unique values)
- **description:**  A short description of the TED talk.  (4005 unique values)
- **transcript:**  Which TED event the talk was given at.  (4005 unique values)


|   talk_id | title                            | speaker_1     | all_speakers         | occupations       | about_speakers                                                                                                                               |   views | recorded_date   | published_date   | event         | native_lang   | available_lang                                                                                                                                                  |   comments |   duration | topics                                       | related_talks                                                                                                                                                                                                                   | url                                                                       | description                                                                               | transcript                                                                                     |
|----------:|:---------------------------------|:--------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|--------:|:----------------|:-----------------|:--------------|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------:|-----------:|:---------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------|:------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------|
|       743 | 10 young Indian artists to watch | Ravin Agrawal | {0: 'Ravin Agrawal'} | {0: ['investor']} | {0: 'As an emerging markets investor, Ravin Agrawal tries to predict the future, balancing economic, political and technological factors. '} |  501623 | 2009-11-06      | 2010-01-20       | TEDIndia 2009 | en            | ['ar', 'bg', 'de', 'el', 'en', 'es', 'fr', 'he', 'hi', 'hr', 'hu', 'it', 'ja', 'ko', 'nl', 'pl', 'pt', 'pt-br', 'ro', 'ru', 'tr', 'uk', 'vi', 'zh-cn', 'zh-tw'] |         35 |        394 | ['Asia', 'art', 'design', 'future', 'india'] | {713: 'Photographing the hidden story', 472: 'My underground art explorations', 643: 'Photographs of secret sites', 1169: 'How I became 100 artists', 1747: 'Embrace the shake', 831: 'How art gives shape to cultural change'} | https://www.ted.com/talks/ravin_agrawal_10_young_indian_artists_to_watch/ | Collector Ravin Agrawal delivers a glowing introduction to 10 of India's most exciting... | Right now is the most exciting time to see new Indian art. Contemporary artists.... (Applause) |


#### Dataset #2:

This dataset was used because it contained views, likes, and video/speech duration data.  

The column breakdown of the dataset (n=5701):

- **talk_id:** Unique id for each individual talk (speech). (5701 unique values)
- **title:**  Title of the talk. (5701 unique values)
- **speaker:**  The speaker giving the talk. (4638 unique values)
- **recorded_date:** The date the talk was recorded. (1901 unique values)
- **published_date:** The date the talk was published on the TED.com website. (3549 unique values)
- **event:**  Which TED event the talk was given at.  (638 unique values) 
- **duration:**  The length (in seconds) of the talk. (1319 unique values)
- **views:**  The number of video views that talk has on the TED website (as of Oct. 13th 2022) (5693 unique values)
- **likes:** Number of likes that video has received on the TED website (as of Oct. 13th 2022) (766 unique values)


|   talk_id | title                      | speaker          | recorded_date   | published_date   | event          |   duration |   views |   likes |
|----------:|:---------------------------|:-----------------|:----------------|:-----------------|:---------------|-----------:|--------:|--------:|
|       723 | My solar-powered adventure | Bertrand Piccard | 2009-07-24      | 2010-01-01       | TEDGlobal 2009 |       1049 |  903784 |   27000 |




## Notebooks

Notebooks are, as of this writing, separated according to which dataset each is doing EDA on.  There are therefore three notebooks thus far:

- A notebook for exploring Dataset #1,
- A notebook for exploring Dataset #2,
- A notebook for merging the likes and views from Dataset #2 into Dataset #1.

- More notebooks are likely to be made.  




## Configuration

The coding environment used for this project has been exported into a .yml file and is included in the main branch of this repository.




## Steps to Complete 
---

- [x] Create initial GitHub repository
  - [x] ReadMe and .gitignore
  - [x] Folder for notebooks with notebooks uploaded
  - [x] Create separate environment, export and upload coding environment details.
- [ ] Preliminary exploratory data analysis ("EDA") on the data
  - [ ] determine feasible measure of "good".
  - [ ] general EDA
- [ ] Merge the likes and views from Dataset #1 into Dataset #2
- [ ] Discover attributes that correlate with what a "good" speech is
- [ ] Begin iterating models


If you have any questions about this project, I would love to speak with you!  Please don't hesitate to reach out:
Phone: +1 778 995 7801
Email: [Drewe.MacIver@gmail.com](mailto:drewe.maciver@gmail.com)
LinkedIn: [Drewe MacIver on LinkedIn](https://www.linkedin.com/in/drewe-maciver/)
