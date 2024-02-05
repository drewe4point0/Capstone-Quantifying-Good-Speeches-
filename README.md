# Capstone: Quantifying Successful Speeches

**Table of Contents:**
- [Introduction, Project Overview](#introduction-project-overview)
- [Problem Area, Those Affected, and Impact](#the-problem-area-and-those-affected)
- [Methodology](#methodology)
- [Data](#data)
- [Notebooks](#notebooks)
- [Configuration](#configuration)
- [Conclusion](#conclusion)




## Introduction, Project Overview

What are the qualities that make a speech impactful?  

This project is to determine insight into what attributes of TED speeches correlate with their success.  It is our hope that insights will be gained for anyone who wishes to impact an audience using speeches, from business leaders giving company town-halls, to students defending their thesis, to keynote speakers looking to "wow" an audience.

This project is also an exercise by it's author (Drewe MacIver) to reasearch and extract insights from these data.  The latter parts of this project will focus on examining the coefficients of regression models (both linear and logistic), and the feature importances of a random forest decision tree classifier, to see what insights can be gained from the features that impact a speech's rating.   

#### Goal:

The goal of this project is to:
1. Determine the attributes of speeches that correlate with that speeches success ("Correlation Analysis" / "CA").
2. Create a prediction model that can predict the rating of a speech ("Prediction Model" / "PM").
3. Gain insights from the coefficients of these predictive models.




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

1. Inital loading of the datasets and aggrregating them into one which I will use.
2. Data cleaning.
3. Feature engineering.
4. Baseline modelling.
5. Iterating over the features of our baseline model to research and/or discover various coefficients.
6. Various other analyses
7. Conclusion


## Data: 

There are two main datasets that are used in the project.  Both were sourced from TED.com, using the [TED Scraper](https://github.com/corralm/ted-scraper/blob/main/README.md), and posted to [Kaggle.com](https://www.kaggle.com/) by individuals that are not connected to this project.  

The first dataset used (sourced from Kaggle.com) can be found [here](https://www.kaggle.com/datasets/miguelcorraljr/ted-ultimate-dataset/data). It's data dictionary can be found below.

The second dataset used (sourced from Kaggle.com) can be found [here](https://www.kaggle.com/datasets/miguelcorraljr/ted-talks-2022). It's data dictionary can be found below.

Special thanks to [Miguel Corral Jr.](https://www.kaggle.com/miguelcorraljr) for compiling both datasets used in this project.  


### Data Dictionaries:

#### Dataset Used (and, separately, the engineered features) (after merging and trimming the two original datasets):

| Column Name      | Rows | Original Datatype | Updated Datatype / Decision             |
|------------------|------|----------|--------------------------------------------------|
| all_speakers     | 3331 | object   | DROPPED / multiple_speakers variable created     |
| occupations      | 3331 | object   | Dummy (via MultiLabelBinarizer)                  |
| about_speakers   | 3331 | object   | DROPPED                                          |
| native_lang      | 3331 | object   | DROPPED                                          |
| available_lang   | 3331 | object   | DROPPED                                          |
| comments         | 3331 | float64  | Ready for analysis                               |
| topics           | 3331 | object   | Dummy (via MultiLabelBinarizer), and CountVectorized in Section #7                  |
| related_talks    | 3331 | object   | DROPPED                                          |
| url              | 3331 | object   | DROPPED                                          |
| description      | 3331 | object   | Possible Future CountVectorization               |
| transcript       | 3331 | object   | CountVectorize                                   |
| title            | 3331 | object   | Possible Future CountVectorization               |
| speaker          | 3331 | object   | DROPPED                                          |
| recorded_date    | 3331 | object   | Changed to DateTime                              |
| published_date   | 3331 | object   | Changed to DateTime                              |
| event            | 3331 | object   | DROPPED / ted_mainstage variable created         |

### Feature Engineering: Features Created from Original Data:

| Feature Created            | Notes                                                         | Datatype |
|----------------------------|---------------------------------------------------------------|----------|
| percent_likes              | likes / views                                                 | float64  |
| ted_mainstage              | mainstage ted events                                          | binary   |
| word_count                 | from the "transcript" column                                  | int64    |
| words_per_minute           | total_words_count / duration                                  | float64  |
| total_question_count       | summing the instances of "?" in the transcript                | int64    |
| questions_per_minute       | total_question_count / duration                               | float64  |
| total_laugh_count          | "Laughter" is included in the transcripts, summing occurrences| int64    |
| laugh_per_minute           | total_laugh_count / duration                                  | float64  |
| multiple_speakers          | whether there were multiple speakers or not                   | binary   |
| published month            | from published_date                                           | int64    |
| published year             | from published_date                                           | int64    |
| recorded month             | from recorded_date                                            | int64    |
| recorded year              | from recorded_date                                            | int64    |

#### Original Dataset #1:

This dataset is used because it contains the transcript for each talk given.  It was last updated on April 30th, 2020, and therefore contains fewer talks than Dataset #2 which was updated on October 13th, 2022.

The column breakdown of the dataset (n=4005):

| Feature          | Description                                                                                             | Unique Values |
|------------------|---------------------------------------------------------------------------------------------------------|---------------|
| talk_id          | Unique id for each individual talk (speech).                                                            | 4005          |
| title            | Title of the talk.                                                                                      | 4005          |
| speaker_1        | The speaker giving the talk.                                                                            | 3274          |
| all_speakers     | If there are multiple speakers, they will be listed here.                                               | 3307          |
| occupations      | The occupations of the speaker(s).                                                                      | 2050          |
| about_speakers   | Details about the speaker's background.                                                                 | 2978          |
| views            | The number of video views that talk has on the TED website (as of April 30th, 2020).                    | 3996          |
| recorded_date    | The date the talk was recorded.                                                                         | 1335          |
| published_date   | The date the talk was published on the TED.com website.                                                 | 2962          |
| event            | The TED event that the talk was given at.                                                               | 459           |
| native_lang      | The language in which the TED talk was given.                                                           | 12            |
| available_lang   | The available languages that the TED talk is available in.                                              | 3902          |
| comments         | The number of comments left on the TED website for that talk.                                           | 602           |
| duration         | The length (in seconds) of the talk.                                                                    | 1188          |
| topics           | The topics that the TED talk covers.                                                                    | 3977          |
| related_talks    | The talk_id and title of other related TED talks.                                                       | 4005          |
| url              | The URL of the TED talk.                                                                                | 4005          |
| description      | A short description of the TED talk.                                                                    | 4005          |
| transcript       | Which TED event the talk was given at. (This seems to be a copy-paste error; possibly the transcript?) | 4005          |



|   talk_id | title                            | speaker_1     | all_speakers         | occupations       | about_speakers                                                                                                                               |   views | recorded_date   | published_date   | event         | native_lang   | available_lang                                                                                                                                                  |   comments |   duration | topics                                       | related_talks                                                                                                                                                                                                                   | url                                                                       | description                                                                               | transcript                                                                                     |
|----------:|:---------------------------------|:--------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|--------:|:----------------|:-----------------|:--------------|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------:|-----------:|:---------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------|:------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------|
|       743 | 10 young Indian artists to watch | Ravin Agrawal | {0: 'Ravin Agrawal'} | {0: ['investor']} | {0: 'As an emerging markets investor, Ravin Agrawal tries to predict the future, balancing economic, political and technological factors. '} |  501623 | 2009-11-06      | 2010-01-20       | TEDIndia 2009 | en            | ['ar', 'bg', 'de', 'el', 'en', 'es', 'fr', 'he', 'hi', 'hr', 'hu', 'it', 'ja', 'ko', 'nl', 'pl', 'pt', 'pt-br', 'ro', 'ru', 'tr', 'uk', 'vi', 'zh-cn', 'zh-tw'] |         35 |        394 | ['Asia', 'art', 'design', 'future', 'india'] | {713: 'Photographing the hidden story', 472: 'My underground art explorations', 643: 'Photographs of secret sites', 1169: 'How I became 100 artists', 1747: 'Embrace the shake', 831: 'How art gives shape to cultural change'} | https://www.ted.com/talks/ravin_agrawal_10_young_indian_artists_to_watch/ | Collector Ravin Agrawal delivers a glowing introduction to 10 of India's most exciting... | Right now is the most exciting time to see new Indian art. Contemporary artists.... (Applause) |


#### Original Dataset #2:

This dataset was used because it contained views, likes, and video/speech duration data.  

The column breakdown of the dataset (n=5701):

| Feature        | Description                                                                                      | Unique Values |
|----------------|--------------------------------------------------------------------------------------------------|---------------|
| talk_id        | Unique id for each individual talk (speech).                                                     | 5701          |
| title          | Title of the talk.                                                                               | 5701          |
| speaker        | The speaker giving the talk.                                                                     | 4638          |
| recorded_date  | The date the talk was recorded.                                                                  | 1901          |
| published_date | The date the talk was published on the TED.com website.                                          | 3549          |
| event          | Which TED event the talk was given at.                                                           | 638           |
| duration       | The length (in seconds) of the talk.                                                             | 1319          |
| views          | The number of video views that talk has on the TED website (as of Oct. 13th 2022).               | 5693          |
| likes          | Number of likes that video has received on the TED website (as of Oct. 13th 2022).               | 766           |



|   talk_id | title                      | speaker          | recorded_date   | published_date   | event          |   duration |   views |   likes |
|----------:|:---------------------------|:-----------------|:----------------|:-----------------|:---------------|-----------:|--------:|--------:|
|       723 | My solar-powered adventure | Bertrand Piccard | 2009-07-24      | 2010-01-01       | TEDGlobal 2009 |       1049 |  903784 |   27000 |




## Notebooks

While earlier iterations of this project containted multiple notebooks, the primary notebook for this dataset is the "Ted Dataset (with Transcripts).ipynb"




## Configuration

The coding environment used for this project has been exported into a BSTN_cap_env_export.yml file and is included in the main branch of this repository.



## Conclusion

### A. General Comment on Modelling and Their Train/Test Scores

I ran logistic regression and random forest decision tree models on many different mixes of features.

I used a variety of CountVectorizers (BagOfWords, with various n_grams and max_features; TFIDF; BERT).  I included various mixes of engineered features (words_spoken_per_minute, laughs_per_minute, questions_per_minute, and others) and dummied features (topic, occupation [of speaker], month_published, year_recorded, and others).  

None of these combinations of features that I tested were able to achieve a train/test score of above roughly 64%/61%.  While this IS predictive, it is not largely so.

The majority of the insights gained in this project stem from looking into the features that had the highest coefficients - especially the CountVectorization bigrams and trigrams, and the numerical engineered features.
  

### B. Topic

The topic of speech research yielded some interesting coefficients.  Topics like Society, Personal Growth, and Work had higher positive coefficients, while topics like Global Issues, Design, and Science had negative coefficients.  (See the tables below).  

While these figures do contribute to a speech receiving more ‘likes’ on the TED website, it should be noted that their coefficients are not especially strong, so this statistic should be taken with a grain of salt.  

For a sense of scale, it should be noted that the topic “society”, and whether or not a speech was given on the “ted_mainstage”, accounted for roughly the same degree of influence, with each having a coefficient of 0.036.  

Logistic Regression (Top 10 Coefficients)
| Feature          | Coefficient |
|------------------|-------------|
| society          | 0.036170    |
| personal growth  | 0.024280    |
| work             | 0.021501    |
| social change    | 0.021344    |
| business         | 0.018442    |
| leadership       | 0.017648    |
| psychology       | 0.017092    |
| humanity         | 0.016840    |
| education        | 0.015957    |
| communication    | 0.015277    |

Logistic Regression (Bottom 10 Coefficients)
| Feature       | Coefficient |
|---------------|-------------|
| Africa        | -0.011622   |
| india         | -0.011786   |
| live music    | -0.012104   |
| war           | -0.014104   |
| technology    | -0.015705   |
| TEDx          | -0.017136   |
| art           | -0.018498   |
| science       | -0.019425   |
| design        | -0.027372   |
| global issues | -0.045442   |




### C. Language Used

The below tables of bigrams are a fair representation of the words that were repeatedly shown to have a positive coefficient and negative coefficient.

At 0.09 to 0.06, the unigrams like “mind”, “students”, “thinking”, and “exactly” have a very high coefficients, relative to the larger n_grams seen in the study, which tended to fall below 0.03.

And while bigrams did have significantly weaker coefficients, we can still learn from their diminished contribution.  General words that hint at a crowd’s or an individual’s **thinking** or **feeling** appeared to positively influence the speech’s rating, while exact words like “90 percent” or “30 years” appeared to negatively influence how that speech was rated.  

If you yourself were looking to have your own speech be more well received, I would encourage you to include the words or phrases that have positive coefficients, and avoid the words or phrases that have negative coefficients. 

Logistic Regression (top 10 positive coefficients - unigrams):
| Feature  | Coefficient |
|----------|-------------|
| mind     | 0.090314    |
| students | 0.081181    |
| thinking | 0.081134    |
| exactly  | 0.078422    |
| doesnt   | 0.077536    |
| instead  | 0.070875    |
| hand     | 0.067619    |
| happen   | 0.067269    |
| computer | 0.066404    |
| number   | 0.064237    |

Logistic Regression (bottom 10 positive coefficients - unigrams):
| Feature  | Coefficient |
|----------|-------------|
| left     | -0.057576   |
| simple   | -0.058305   |
| book     | -0.059713   |
| working  | -0.061128   |
| months   | -0.061594   |
| took     | -0.063037   |
| bring    | -0.071974   |
| decided  | -0.072279   |
| build    | -0.080311   |
| im going | -0.085750   |


Logistic Regression (top 10 positive coefficients - bigrams):
| Feature       | Coefficient |
|---------------|-------------|
| want know     | 0.039126    |
| people say    | 0.029091    |
| people think  | 0.026444    |
| feels like    | 0.026033    |
| dont think    | 0.025069    |
| black hole    | 0.024175    |
| know people   | 0.022336    |
| years later   | 0.020470    |
| high school   | 0.020177    |
| answer question| 0.020100    |

Logistic Regression (top 10 negative coefficients - bigrams):
| Feature          | Coefficient |
|------------------|-------------|
| 90 percent       | -0.017563   |
| 30 years         | -0.018234   |
| middle east      | -0.018441   |
| ♫ ♫              | -0.019905   |
| im going         | -0.023090   |
| just want        | -0.023170   |
| ladies gentlemen | -0.025534   |
| looks like       | -0.025968   |
| people living    | -0.029898   |
| weve got         | -0.031898   |

Random Forest Feature Importances - quadgrams:
| Feature                    | Importance |
|----------------------------|------------|
| weve come long way         | 0.108924   |
| thank thank thank thank    | 0.105864   |
| small thing big idea       | 0.092390   |
| thank chris anderson thank | 0.084021   |
| im going talk today        | 0.076134   |
| id like talk today         | 0.063073   |
| let tell little bit        | 0.056309   |
| make world better place    | 0.049534   |
| dont know dont know        | 0.047605   |
| today im going talk        | 0.046653   |



### D. Numerical Engineered Features

The highest degree of influence on whether a speech was rated above the mean or not was whether it was given on the “ted_mainstage”.  

The “laughs_per_minute” and the number of website “comments” seemed to have a similar degree of effect on a speeches rating.  

Interestingly, the rate of speech (the “words_per_minute”) was the weakest coefficient of these variables.  It seems that the audience does rate a speech much more favourably if the speaker talks quickly or slowly.  

I encourage you to look at the tables below and see for yourself what you find interesting about these findings!

Logistic Regression:
| Feature               | Coefficient |
|---------------------|------------|
| ted_mainstage       | 0.169018   |
| num_question_marks  | 0.088434   |
| comments            | 0.074825   |
| laughs_per_minute   | 0.074142   |
| questions_per_minute| 0.070605   |
| num_laughs          | 0.061112   |
| word_count          | 0.036111   |
| duration            | 0.032129   |
| words_per_minute    | 0.028710   |
| multiple_speakers   | -0.049351  |


Random Forest:
| Feature             | Importance |
|---------------------|------------|
| comments            | 0.28       |
| laughs_per_minute   | 0.16       |
| questions_per_minute| 0.12       |
| num_laughs          | 0.10       |
| words_per_minute    | 0.08       |
| num_question_marks  | 0.08       |
| duration            | 0.06       |
| ted_mainstage       | 0.06       |
| word_count          | 0.06       |
| multiple_speakers   | 0.00       |


### E. Title

While the length of a speech’s title didn’t appear to correlate much with the percent_likes it received (R^2 value of 0.016, and a Coefficient of 0.0877), some interesting coefficients were found in the words that a title contains.  Words like “Teach”, “Build”, and “Future” had a positive impact on the speeches likes, while words like “making”, “lives”, and “women” had a negative impact.  

Logistic regression (top 10 positive coefficients):
| Feature | Coefficient |
|---------|-------------|
| teach   | 0.059401    |
| build   | 0.053227    |
| future  | 0.051909    |
| learned | 0.049527    |
| love    | 0.048637    |
| better  | 0.047232    |
| 3       | 0.045885    |
| power   | 0.040386    |
| work    | 0.039140    |
| tell    | 0.038766    |


Logistic regression (top 10 negative coefficients):
| Feature | Coefficient |
|---------|-------------|
| making  | -0.035987   |
| lives   | -0.036566   |
| women   | -0.037411   |
| ocean   | -0.038510   |
| science | -0.039195   |
| new     | -0.041241   |
| cancer  | -0.042177   |
| music   | -0.050583   |
| art     | -0.061308   |
| global  | -0.065322   |



### F. Ted MainStage Impact

Speeches given at MainStage TED events (and not TEDx, or other offshoot events) did receive a higher rating, but not by much.  When normalized, the mean difference in percent_likes was 0.012% higher for mainstage speeches than it was for non-mainstage ones.  Statistically significant, but not major.   




### In Summary

In summary, I have learned that there appear to be features of a speech that do empirically positively impact how well that speech is received.  The “laughs_per_minute” is 2.5x more influential than how fast the presenter speaks (“words_per_minute”).  

Words like “mind”, “students”, “thinking”, and “exactly” had very strong influences relative to other words and/or features.  

General words like “want know”, “people say”, “people think”, and “feels like” had a less strong (compared to the above single-word features) but still positive influence throughout this research.  And words that were more precise, such as “90 percent” or “30 years” had a negative influence.  

And whether or not a talk was given on the “ted_mainstage” had the most influence out of all the features, but while this influence was most significant, this ‘most significant’ feature still only led to a relative 0.012% degree of influence on how many ‘likes’ a talk received on the TED website.  

It has been a pleasure researching this topic, and I look forward to doing more research into what else can influence the impact that a speech can have on its audience.  


If you have any questions about this project, I would love to speak with you!  Please don't hesitate to reach out:
Phone: +1 778 995 7801
Email: [Drewe.MacIver@gmail.com](mailto:drewe.maciver@gmail.com)
LinkedIn: [Drewe MacIver on LinkedIn](https://www.linkedin.com/in/drewe-maciver/)
