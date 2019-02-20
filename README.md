# TransferWise - How to Find Your Target Audience
-  What positive keywords help us reach digital nomads?
-  What negative keywords help us avoid other type of travelers?

## Problem Statement
### Background: 

TransferWise is a UK-based money transferring service that allow users to hold and exchange multiple currencies at once. The exchange rate and fees associated with the service is also much lower than traditional banks, making it incredibly beneficial for expatriates. In order to effectively use Google Display Network for a stronger US market presence, TransferWise will leverage one common targeting method: keywords, when developing a display network campaign. 

Keywords are words or phrases that can be set to determine when and where digital advertisements appear. There are two types: 
-	**Positive**: Ads will be placed on sites that contain content with positive keywords.

-	**Negative**: Negative keywords allow us to choose what *not* to target. The terms may be similar to positive keywords, but cater to a different customer base.  Ads will not be placed on sites with content that contain these words.
Setting both positive and negative keywords will increase the likelihood of a company targeting the right demographic and, in turn, increase customer conversion rate.  

Including a substantial amount of keywords will refine our campaign considerably, but can be too costly to implement. To determine the most effective keywords to include, I analyzed two subreddit communities and the words most commonly used amongst them:

1. **r/digitalnomad**: Most users in this community are either digital nomads or aspire to be one. These individuals typically live abroad long-term, and are the type of customers we want to target. Words that appear frequently here can be effectively used as positive keywords.

2.	**r/solotravel**: This community primarily consists of individuals looking to travel short-term for leisure. Even though there may be some overlap, we do not want to exhaust our resources targeting these customers because our service are not as beneficial in these cases.  Words that appear frequently here can be effectively used as negative keywords. 

### Approach:
#### 1.	Data Collection:
-  The most recent posts were collected from r/digitalnomad and r/solotravel, with a maximum set at 6,000. r/solotravel is relatively more active and, as a result, has more posts in a shorter amount of time. To undersample our majority class and prevent class imbalance, 6,000 posts were collected from r/solotravel from August 10, 2018 – December 19, 2018. 5,705 posts were collected from r/digitalnomad from January 1, 2018 – December 19, 2018. 

#### 2.	Exploratory Data Analysis and Cleaning:
-	We cleaned the data to only include information relevant to our model, `title` and `selftext`. After dropping posts that were either previously removed by the moderator or the user in the community, we have a corpus of 10, 767 documents. 
-	The following were removed either because they only contributed noise or were not relevant to our goal of determining the most effective keywords:
    -  links
    -  subreddit name
    -  distinct words: digital nomad, solo travel and variations of them
    -  HTML code artifacts
    -  digits
-	Trends between and within the two subreddits were explored to quickly see which words were most prevalent. 

#### 3.	Preprocessing and Modeling:
-	**Baseline Model**
    -  Accuracy score = 52.47% 
    -  r/solotravel is our majority class and if we classify all posts to belong in this subreddit, we will be predicting correctly 52.47% of the time.
-	**Logistic Regression (Production Model)**
    -  Accuracy score = 81.35%
    -  The model performs significantly better than our baseline model of 52.47%.
    -  We can quickly determine which terms are highly associated with r/digitalnomad and r/solotravel and assign them to be either a positive or negative keyword. 
    -  The model performs poorly with posts that contain a combination of words with high and low odds of occurring in r/digitalnomad.  It’ll be beneficial to adjust the threshold once we know whether to optimize for sensitivity or specificity. 
-	**Random Forest**
    -  Accuracy score = 81.17%
    -  The model performs significantly better than our baseline model of 52.47%. 
    -  The model performs slightly worse than Logistic Regression, with the test score being lower by 0.18%.
    -  We’re able to measure feature importance, but cannot easily discern whether a term should be used as a positive or negative keyword. 

### Model Comparison:
**Advantages with Logistic Regression:**
-  We can easily determine how likely a post will be classified in r/digitalnomad by taking the exponential of the log odds of success. Even though Random Forest informs us about feature importance, we cannot quickly determine the information gained classifies a post in which class. 
-  Relative to Random Forest, our model is not overfitting. The training score is 1.89% higher than our test score, compared to 14.48%. 
-  There are less false negatives (215) than random forest (250). It may be more beneficial to mistakenly target indviduals who are not digital nomads than to overlook someone who is.

**Advantages with Random Forest:**
-  We have a slightly higher accuracy score of 81.76%. 
-  There are less false positives (256) than logistic regression (286). We are cutting back on costs that we may incur from targeting individuals who do not find our service useful.

### Conclusions and Recommendations

**Use Logistic Regression as Production Model:** With a slightly higher score than Random Forest, Logistic Regression proves to be a more successful and effective model for our purposes. Ultimately, we want to know which words should be used as positive keywords and which should be used as negative keywords to find our target audience. Logistic regression easily provides this distinction with the exponential log odds of success. While Random Forest identifies these keywords, the distinction between positive and negative is not as readily apparant. 

|Positive Keywords|Negative Keywords|
|---|---|
|remote|trip|
|internet|hostel|
|business|itinerary|
|work|booked|

**Positive Keywords:** Websites that contain either one or a combination of these words will be ideal targets for our campaign. Posts with these words have high odds of being in r/digitalnomad, which are exactly the users we're trying to target. When discussed with travel, these words also imply it's not for leisure. Digital nomads will typically be more concerned with internet connection and work, because they ultimately still have work-related responsibilities depsite being in a foreign area.

**Negative Keywords:** To ensure we're allocating resources in the right places, we want to specify these negative keywords and avoid having advertisements on these websites. Posts with these words have low odds of being in r/digitalnomad, which means it concerns a demographic that we have little interest in. All of these words indicate short-term travelers, who will find little to no benefit in our service.

**Next Steps:** Once a campaign budget is determined and we know the cost-benefit trade off, we can return to our production model and optimize for sensitivity or specificity. Without knowing all the costs involved, it's difficult to determine whether false positives or false negatives should be minimized. 

## Executive Summary

### Contents:
**1\. Data Collection**
-  Digital Nomad Data Collection
-  Solo Travel Data Collection
-  Save as JSON

**2\. Exploratory Data Analysis and Cleaning**
-  Digital Nomad Data Import and Cleaning
-  Solo Travel Data Import and Cleaning
-  Combine Digital Nomad and Solo Travel
-  Clean Combined Text
-  Data Visualization
   -  Word and Character Count
   -  Most Common Words
-  Save as CSV

**3\. Preprocessing and Modeling**
-  Import Data
-  Model Prep
-  Modeling
   -  Baseline Model
   -  Logistic Regression
       -  Odds
       -  Predictions and Probabilities
   -  Random Forest
       -  Important Features
       -  Predictions
-  Model Comparison
-  Conclusions and Recommendations

### Data Source
-  [Reddit - Digital Nomad](https://www.reddit.com/r/digitalnomad/)
-  [Reddit - Solo Travel](https://www.reddit.com/r/solotravel/)