# Data-Wrangling-in-Python

## Data wrangling process.

> Data Gathering.

> Data Assessing.

> Data Cleaning.

### 1.0 Data Gathering
The 'We Rate Dogs' projects had three data sources.

1. twitter_archive_enhanced.csv is a file which I downloaded manually and stored in my working directory.
2. image_predictions is the second file which i downloaded from Udacity servers using the requests library.
3. For the JSON data, I downloaded by querying the Twitter API using the Tweepy library. I extracted favourite_count and retweet_count programmatically 
from the tweet_json.txt file.


### 2.0 Assessment
I used both visual and programmatic methods of assessment. For Visual assessment, I opened the files in other softwares like Excel and scrolled through the numerous 
rows noting data inconsistencies.

For programmatic assessment, I used my jupyter notebook to assess data quality and data tidiness issues. 
For data quality, I was looking foe completeness, validity, accuracy and consistency of the three dataframes. 
For data tidiness, I was looking for structural issues.

#### 2.1 Assessment findings

##### 2.1.1. Quality
###### archive
I assessed archive, predictions and tweet_stats data systematically.

The project required the use of original tweets only. The first assement task was to find out whether the archive dataframe contained retweets and reply tweets. I found that archive contained 181 retweets andd 78 reply tweets.

The rating_numerator columns had values that were too large that were either inaccurately typed or were extreme outliers.

The rating_denominator had outliers and values that weren't the base 10.

The expanded urls column had some missing data. I however ignored the missing data because they were very few and wouldn't affect the analysis.

Timestamp column had an erroneous datatype. It was object/string instead of datetime.

The source column had ambigous text: a html tag.

###### predictions
The jpg_url column had 66 duplicate images.

###### tweet_stats
The date_created column has an erroneous datatype.

##### 2.1.2 Tidiness
###### archive
The assessment revealed that there were four columns doggo, puppo, pupper, floffer which are all dog stages.

###### prediction
p1,p2,p3 are columns containing the same data: (breed) this will be determined by the highest p_conf among p1_conf, p2_conf, and p3_conf.

There are other unnecessary columns that won't assist in the analysis.

## 3.0 Cleaning
### making a copy of the data
Before cleaning, I made copies of my data. I named the dataframes: archive_clean, predictions_clean and tweet_stats_clean respectively.

###### archive_clean
I began cleaning archive_clean. I dropped 181 tweets that were retweets and the 78 that were replies.

Since I no longer had retweets and replies in my dataframe, I dropped 'retweeted_status_id', 'retweeted_status_user_id', 'retweeted_status_timestamp', 'in_reply_to_status_id', and 'in_reply_to_user_id'columns.

I converted timestamp's datatype from string to datetime using pd.to_datetime.

I then extracted the device used to tweet from the source column using regualar expressions.

I melted the four dog stages: doggo, floofer, pupper, puppo into one column which I named dogs_stage. In addition, I sorted dogs_stage in order to then drop duplicated based on tweet_id except for the last occurrence.

I filtered rating_numerator column to have only ratings < 20

I filtered rating_deniominator to only have a denominator == 10

Since the rating_denominator column only contained a 10 rating, It was no longer useful. I therefore dropped it.

I renamed the rating_numerator column to rating

###### predictions_clean
I created two new columns, breed and confidence. breed was determined on condition of having the max p_conf. Confidence column contains the max conf.

I dropped the duplicate jpg_url images then dropped columns that were no longer useful.

###### tweet_stats_clean
I changed the date_created datatype from string to date time.

## Wrangling conclusion
Finally I merged the three cleaned dataframes on tweet_id and saved as a csv named twitter_archive_data.
