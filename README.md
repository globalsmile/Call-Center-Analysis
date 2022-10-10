# Pwc Switzerland Power BI virtual case experience - Call Center Analysis
<img align="right" alt="Call Center Analysis" width="1000" height = "400" src="https://www.pwc.ch/en/images/blog/virtual-case-experience/HC_Virtual%20Case%20Experience_1000x560_NWNS-Power%20BI.png">

---


# Table of Contents

- [Problem Statement](https://github.com/globalsmile/Call-Center-Analysis#Problem-Statement)
- [Data Sourcing](https://github.com/globalsmile/Call-Center-Analysis#Data-Sourcing)
- [Data Preparation](https://github.com/globalsmile/Call-Center-Analysis#Data-Preparation)
- [Data Modeling](https://github.com/globalsmile/Call-Center-Analysis#Data-Modeling)
- [Data Visualization](https://github.com/globalsmile/Call-Center-Analysis#Data-Visualization)
- [Data Analysis](https://github.com/globalsmile/Call-Center-Analysis#Data-Analysis)
- [Insights](https://github.com/globalsmile/Call-Center-Analysis#Insights)
- [Shareable link](https://github.com/globalsmile/Call-Center-Analysis#Shareable-Link)


---

# Problem Statement

INTRO :This project is one of three projects from the [Pwc Switzerland Power BI virtual case experience internship program](https://www.theforage.com/virtual-internships/prototype/a87GpgE6tiku7q3gu/PwC-Digital-Up-skilling-Virtual-Case-Experience) on Forage.

The purpose of this analysis is to create create a dashboard for the call center manager that reflects all relevant Key Performance indicators(KPIs)
and metrics in the dataset.

Possible KPIs include(but not limited to):
- Overall customer satisfaction
- Overall calls answered/abadoned
- Calls by time
- Average speed of answer
- Agents performance quadrant -> average handle time(talk duration) vs calls answered


---

# Data Sourcing

- The dataset used for this analysis was scrapped from twitter using python and jupyter notebook
- The preview of the dataset and python code is shown below:


```python
!pip install snscrape
```

    Requirement already satisfied: snscrape in c:\users\user\anaconda3\lib\site-packages (0.4.3.20220106)
    Requirement already satisfied: lxml in c:\users\user\anaconda3\lib\site-packages (from snscrape) (4.8.0)
    Requirement already satisfied: beautifulsoup4 in c:\users\user\anaconda3\lib\site-packages (from snscrape) (4.11.1)
    Requirement already satisfied: requests[socks] in c:\users\user\anaconda3\lib\site-packages (from snscrape) (2.27.1)
    Requirement already satisfied: filelock in c:\users\user\anaconda3\lib\site-packages (from snscrape) (3.6.0)
    Requirement already satisfied: soupsieve>1.2 in c:\users\user\anaconda3\lib\site-packages (from beautifulsoup4->snscrape) (2.3.1)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (3.3)
    Requirement already satisfied: charset-normalizer~=2.0.0 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (2.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (1.26.9)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (2021.10.8)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (1.7.1)
    


```python
import pandas as pd
import snscrape.modules.twitter as sntwitter
```


```python
query = "(#30DaysOfLearning OR #NG30DaysOfLearning) until:2022-06-26 since:2022-05-05"
tweets = []
limit = 30000


for tweet in sntwitter.TwitterHashtagScraper(query).get_items():
    
    if len(tweets) == limit:
        break
    else:
        tweets.append([tweet.date, tweet.url, tweet.user.username, tweet.sourceLabel, tweet.user.location, tweet.content, tweet.likeCount, tweet.retweetCount,  tweet.quoteCount, tweet.replyCount])
        
df = pd.DataFrame(tweets, columns=['Date', 'TweetURL','User', 'Source', 'Location', 'Tweet', 'Likes_Count','Retweet_Count', 'Quote_Count', 'Reply_Count'])

df.to_csv('30DLTweets.csv')
```


```python
df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>TweetURL</th>
      <th>User</th>
      <th>Source</th>
      <th>Location</th>
      <th>Tweet</th>
      <th>Likes_Count</th>
      <th>Retweet_Count</th>
      <th>Quote_Count</th>
      <th>Reply_Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-06-25 22:51:18+00:00</td>
      <td>https://twitter.com/poetrineer/status/15408300...</td>
      <td>poetrineer</td>
      <td>Twitter for Android</td>
      <td>Oyo, Nigeria</td>
      <td>So as one of my commitment to document my lear...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-06-25 22:44:10+00:00</td>
      <td>https://twitter.com/poetrineer/status/15408282...</td>
      <td>poetrineer</td>
      <td>Twitter for Android</td>
      <td>Oyo, Nigeria</td>
      <td>Finally, here is my updated COVID-19 Data Anal...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-06-25 19:25:58+00:00</td>
      <td>https://twitter.com/MichealOjuri/status/154077...</td>
      <td>MichealOjuri</td>
      <td>Twitter Web App</td>
      <td>Oyo, Nigeria</td>
      <td>#30NGDaysOfLearning\n#30daysoflearning \n#micr...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-06-25 16:44:36+00:00</td>
      <td>https://twitter.com/oye__aashu/status/15407377...</td>
      <td>oye__aashu</td>
      <td>Twitter for Android</td>
      <td>Nainital, India</td>
      <td>Day 4/ #30daysoflearning learned all about arr...</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-06-25 12:49:02+00:00</td>
      <td>https://twitter.com/hsb_data/status/1540678455...</td>
      <td>hsb_data</td>
      <td>Twitter Web App</td>
      <td>New Jersey</td>
      <td>Learning about sub queries on @DataCamp (SQL) ...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Likes_Count</th>
      <th>Retweet_Count</th>
      <th>Quote_Count</th>
      <th>Reply_Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>15.780381</td>
      <td>3.812592</td>
      <td>0.185944</td>
      <td>1.166911</td>
    </tr>
    <tr>
      <th>std</th>
      <td>41.164555</td>
      <td>12.665561</td>
      <td>0.737938</td>
      <td>2.626554</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>8.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>549.000000</td>
      <td>248.000000</td>
      <td>9.000000</td>
      <td>29.000000</td>
    </tr>
  </tbody>
</table>
</div>




The dataset is also available at [30DLTweets](https://github.com/globalsmile/Twitter-Sentiment-Analysis/blob/main/30DLTweets.csv)

---

# Data Preparation

Data transformation was done in Power Query and the dataset was loaded into Microsoft Power BI Desktop for modeling.

The call center dataset is contained in a table named:

- `Call Center` which has `10 columns and 5000 rows` of observation


The tabulation below shows the `Call Center` table with its column names and their description:
| Column Name | Description |
| ----------- | ----------- |
| Call Id | Represents every unique observation in the dataset |
| Agent | Describes the name of the agent |
| Date | Describes the date of the call |
| Time | Represents the time of the call  |
| Topic | Describes the topic of the caller |
| Answered (Y/N) | Describes if the call was Answered or not |
| Resolved | Describes if the problem was Resolved or not |
| Speed of answer in seconds | Represents the speed of answer in seconds |
| AvgTalkDuration | Represents the average talk duration of call |
| Satisfaction rating | Represents the satisfaction rating of the agent during the call |

Data Cleaning for the dataset was done in power query as follows:

- Unnecessary columns were removed 
- Each of the columns in the table were validated to have the correct data type
- Unnecessary rows were removed 

To ensure the accuracy of the dates in the `Date` column of `Call Center` table, a date table was created for referencing using the M-formula:

`{Number.From(List.Min(Call Center[Date]))..Number.From(List.Max(Call Center[Date]))}`

Here is a breakdown of what the formula does:

For the dataset, we want the start date to reflect the earliest to latest date that we have in the data: January 1, 2021 - March 31, 2021.

The date table was named `Calender`.

---

# Data Modeling

After the dataset was cleaned and transformed, it was ready to be modeled.

- The `Calender` table was marked as the official date table in the dataset.
- A one-to-many (*:1) relationship was created between the `Call Center` and the `Calender` tables using the date column in each of the tables
- The realtioship formed in the data model is shown below:

<img align="right" alt="Data Model" width="1000" height = "400" src="https://user-images.githubusercontent.com/106287208/187106255-bd25a422-fd74-4f5e-a529-7a9052915252.png">


---

# Data Visualization

Data visualization for the datasets was done in Microsoft Power BI Desktop:

-  The `Call Center Manager` Page: Shows KPIs including overall customer satisfaction, overall calls answered/abadoned, calls by time, average speed of answer, agents performance quadrant -> average handle time(talk duration) vs calls answered


Figure 1 shows visualizations from `Call Center Manager` page

| Figure 1 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/187567316-46bc6332-7507-4f11-b3c7-a18a52ed8e14.png) |


---

# Data Analysis

Measures used in visualization are:

- # of answered = `Calculate(distinctcount('Call Center'[Call Id]),Filter('Call Center','Call Center'[Answered (Y/N)]="Y"))`
- # of resolved = `Calculate(distinctcount('Call Center'[Call Id]),Filter('Call Center','Call Center'[Resolved]="Y"))`
- Average satisfaction rating = `Average('Call Center'[Satisfaction rating])`
- Average Speed of answer = `Average('Call Center'[Average Speed of anser in seconds])`
- Target Value Satisfaction = `4.5`


As shown from [Data Visualization](https://github.com/globalsmile/Call-Center-Analysis#Data-Visualization), It can be deduced that:

- The average satisfation rating is `3.40` 
- `81.08%` of total calls were answered and `18.92%' of total calls were not answered
- `72.92%` of total calls were resolved and `127.08%' of total calls were not resolved

---

# Insights

As shown by [Data Visualization](https://github.com/globalsmile/Twitter-Sentiment-Analysis#Data-Visualization), It can be deduced that:

- In January a total of `1455` calls were answered, whereas `317` calls were not answered 
- In February a total of `1298` calls were answered, whereas `318` calls were not answered 
- In March a total of `1301` calls were answered, whereas `311` calls were not answered 
- The average speed of answer is `67.52` seconds

---

# Shareable Link

You can interact with the report here: 

[Report](https://app.powerbi.com/view?r=eyJrIjoiZjMzMjk1ZDAtYzBjYy00OTZjLTk1YzQtMzI1MjE0NWFkOGYxIiwidCI6IjQ5ODY4YWYzLWNjNWYtNDIxNC04YjdmLTQwZjM3NDY0OWEwOSJ9)
