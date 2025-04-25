# INSTRUCTION OVERVIEW: YouTube API Analysis, OMNI Filipino News' 2024 Coverage (with Python)

<P>This document outlines how to run this project. It does not need to be limited to OMNI News Filipino Edition, as this could be executed for other editions of OMNI News, or even any channel not related to ethnic media.</P>

## Table of Contents
* [Code and Resources Used](#code-and-resources-used)
* [Part 1: Scraping Channel and Video Data Using YouTube API](#part-1-scraping-channel-and-video-data-using-youtube-api)
  * [Step 1: Get channel's ID](#step-1-get-channels-id)
  * [Step 2: Get channel data](#step-2-get-channel-data)
  * [Step 3: Get playlist ID](#step-3-get-playlist-id)
  * [Step 4: Ascertain Video Details of the playlist](#step-4-ascertain-video-details-of-the-playlist)
* [Part 2: Data Cleaning and Pre-Processing Using Pandas](#part-2-data-cleaning-and-pre-processing-using-pandas)
  * [Step 1: Pandas Operations for "Likes", "Comments", "Views"](#step-1-pandas-operations-for-likes-comments-views)
  * [Step 2: Ensure Publication Dates are Stored as Dates](#step-2-ensure-publication-dates-are-stored-as-dates)
  * [Step 3: Drop Duplicated Titles](#step-3-drop-duplicated-titles)
  * [Step 4: Filter for 2024 Videos](#step-4-filter-for-2024-videos)
  * [Step 5: Group by Months to Perform Month-by-Month Operations](#step-5-group-by-months-to-perform-month-by-month-operations)
  * [Step 6: Top 5 for Each Metric for 2024](#step-6-top-5-for-each-metric-for-2024) 
* [Part 3: Data Visualization with HV Plot](#part-3-data-visualization-with-hv-plot)
  * [Step 1: Install Necessary Libraries](#step-1-install-necessary-libraries)
  * [Step 2: Create First Panel Visualization (Monthly Statistics)](#step-2-create-first-panel-visualization-monthly-statistics)
  * [Step 3: Create Second Panel Visualization (Top 5/Top 10 Tables)](#step-3-create-second-panel-visualization-top-5top-10-tables)
  * [Step 4: Histogram Visualization (Views)](#step-4-histogram-visualization-views)
  * [Step 5: Tagalog Language Natural Language Processing](#step-5-tagalog-language-natural-language-processing)

<details><summary><h2>Code and Resources Used</h2></summary> 
  <ul>
    <li><b>IDEs Used:</b> Google Colab, Jupyter Notebook</li>
    <li><b>Python Version:</b> 3.10.12</li>
    <li><b>Libraries and Packages:</b>
    <ul>
      <li><b>API Libraries: </b> YouTube (from pytube), googleapiclient.discovery, json, JSON (from IPython.display) </li>
      <li><b>Libraries for data manipulation and visualization: </b> pandas, datetime, numpy, hvplot, panel</li>
      <li><b>Libraries for natural language processing: </b> Image (from PIL), seaborn, matplotlib, WordCloud (from wordcloud), STOPWORDS (from wordcloud), ImageColorGeneratorplotly.express (from wordcloud)</li>
    </ul></li>
  </ul>

</details>

<details><summary><h2>Part 1: Scraping Channel and Video Data Using YouTube API</h2></summary> 

#### Step 1: Get channel's ID

<p>To get any channel's ID, you'll have to click "share channel" in the "About" section, then you will see a button that will say "copy channel ID".</p>

![image](https://github.com/user-attachments/assets/b578f53e-94fe-4f29-b6fc-9695c3f9a4d1)

<p>After copying it, paste it in your notebook and set is as a variable for convenience.</p>

```python
# Get channel id of OMNI Television's YouTube channel
channel_ids=['UC_8NvxZFBoG5vtco9_TvYfw',]
```

#### Step 2: Get channel data

<p>Use the following two codes in sequential order in order to scrape the channel's data using the channel ID. Much of the code snippets are actually found in the YouTube API website (https://developers.google.com/youtube/v3/docs/channels/list):</p>

```python
# Most of the code can be found from https://developers.google.com/youtube/v3/docs/channels/list

api_service_name = "youtube"
api_version = "v3"

# Get credentials and create an API client
youtube = googleapiclient.discovery.build(
        api_service_name, api_version, developerKey=api_key)

request = youtube.channelSections().list(
        part="snippet,contentDetails",
        channelId=','.join(channel_ids)
    )
response = request.execute()

response=json.dumps(response)
json.loads(response)
```
```python
# Code to scrape channel statistics and display them as pandas dataframe
def get_channel_stats(youtube, channel_ids):
    all_data = []
    request = youtube.channels().list(
        part="snippet,contentDetails,statistics",
        id=','.join(channel_ids)
    )
    response=request.execute()

    #loop through items
    for item in response['items']:
        data = {'channelName': item['snippet']['title'],
                'subscribers': item['statistics']['subscriberCount'],
                'views': item['statistics']['viewCount'],
                'totalVideos': item['statistics']['videoCount'],
                'playlistId': item['contentDetails']['relatedPlaylists']['uploads']
                }

        all_data.append(data)

    return pd.DataFrame(all_data)
     
```

<p>We now have our channel data:</p>

![image](https://github.com/user-attachments/assets/ca71405b-ee29-47db-bf2e-8adb1ecfef0a)

#### Step 3: Get playlist ID

<p>To get the playlist ID, go to the playlist of interest, click "share", and you will see its url. The ID is located in the url, after "list=": </p>

![image](https://github.com/user-attachments/assets/b369c794-0919-4589-bfd6-bc6e28c3d364)

<p>Then use the following code snippet (taken from https://developers.google.com/youtube/v3/docs/playlists/list):</p>

```python
# Now, using code from https://developers.google.com/youtube/v3/docs/playlists/list,
# scrape data from OMNI Television's Filipino News playlist
# We only need the request and response portion

request = youtube.playlists().list(
        part="snippet,contentDetails",
        id="PLpYhyoAjmlDhnInWbukk9LAiqwRob6DGE" # playlist ID
    )
response = request.execute()
print(response)
```

#### Step 4: Ascertain Video Details of the playlist

<p>First, set the playlist ID as a variable for convenience:</p>

```python
playlist_id='PLpYhyoAjmlDhnInWbukk9LAiqwRob6DGE'
```

<p>Then use the following code snippet that partially draws from this webpage (https://developers.google.com/youtube/v3/docs/videos/list): </p>

```python
# Create a function to scrape video ids of all videos in the playlist
# Use parts of the code from https://developers.google.com/youtube/v3/docs/videos/list
# and embed in the function
# We only need the request and response portion
def get_video_ids(youtube, playlist_id):

    video_ids = []
    request = youtube.playlistItems().list(
        part="snippet,contentDetails",
        playlistId=playlist_id,
        maxResults = 50 # The set maximum is 50, but we want all of the videos
    )
    response = request.execute()

    for item in response['items']:
        video_ids.append(item['contentDetails']['videoId'])

    # To get around the 50-item limitation, we will need to insert the following
    # for loop in between this function
    # Use the same code as above
    ##################################################################################

    next_page_token = response.get('nextPageToken')
    while next_page_token is not None:
      request=youtube.playlistItems().list(
        part="snippet,contentDetails",
        playlistId=playlist_id,
        maxResults = 50,
        pageToken=next_page_token
      )
      response = request.execute()

      for item in response['items']:
        video_ids.append(item['contentDetails']['videoId'])

      next_page_token = response.get('nextPageToken')

    # This loop essentially retieves data for the next 50 results (i.e., videos),
    # and iterates until the function has wnet through the entire playlist
    ##################################################################################

    return video_ids
```

<p>To check if the function works, check if the total number of entries ("length") matches the total number of videos uploaded, which it indeed does (as of the completion date of this project): </p>

```python
video_ids=get_video_ids(youtube, playlist_id)
len(video_ids) # Check if the length matches the number of videos in the playlist
```

<p>Now we can use this code snippet to get video statistics, partially drawing upon this webpage (https://developers.google.com/youtube/v3/docs/videos/list): </p>

```python
# Now we can get the statistics of each video in the playlist
# Use code from https://developers.google.com/youtube/v3/docs/videos/list
# We only need the request and response portion

def get_video_details(youtube, video_ids):
  all_video_info = []

  # Loop through video_ids in chunks of 50
  for i in range(0, len(video_ids), 50):
    request = youtube.videos().list(
        part="snippet,contentDetails,statistics",
        id=','.join(video_ids[i:i+50])
    )
    response = request.execute()
  # Get stats
    for video in response['items']:
        stats_to_keep = {'snippet': ['channelTitle', 'title', 'description', 'tags', 'publishedAt'],
                        'statistics': ['viewCount', 'likeCount', 'favouriteCount', 'commentCount'],
                        'contentDetails': ['duration', 'definition', 'caption']
                        }
        video_info = {}
        video_info['video_id'] = video['id']

        for k in stats_to_keep.keys():
            for v in stats_to_keep[k]:
                try:
                    video_info[v] = video[k][v]
                except:
                    video_info[v] = None # This is to account for any null values
        all_video_info.append(video_info)

  # Moved return statement outside the loop to return the complete DataFrame
  return pd.DataFrame(all_video_info)
```

<p>The result: </p>

```python
video_df=get_video_details(youtube, video_ids)
video_df
# For some reason, a video from OMNI News Mandarin was included in the playlist, perhaps by mistake, disregard
```
![image](https://github.com/user-attachments/assets/588b8783-e50d-463e-8947-89b2f3c0ce04)

</details>

<details><summary><h2>Part 2: Data Cleaning and Pre-Processing Using Pandas</h2></summary> 

#### Step 1: Pandas Operations for "Likes", "Comments", "Views"

<p>The three mentioned variables constitute a video's engagement. We perform a check to ensure that there are no null values, as that could cause issues.</p>

<p>Then, we convert those three columns into "numeric" columns, as in the dataframe, even though the counts obviously represent numerical values, it is currently stored as "strings" (or, treated as "words").</p>

#### Step 2: Ensure Publication Dates are Stored as Dates

<p>In a similar vein, we want the dates to be stored as dates, not as strings. Therefore, this pandas operation was executed to convert their datatype:</p>

```python
# Convert "publishedAt" string column to datetime format
PublishDate=video_df['publishedAt']
PublishDate_String = pd.to_datetime(video_df['publishedAt'])
PublishDate_Formatted = PublishDate_String.dt.strftime('%Y-%m-%d')
PublishDate_Formatted
```

<p>The column can then be added to the dataframe.</p>

#### Step 3: Drop Duplicated Titles

<p>Duplicated entries in the "title" column represents duplicate uploads and are not useful for this analysis. Check for any duplicates in that columns, then drop their associated rows.</p>

#### Step 4: Filter for 2024 Videos

<p>Use this code snippet to filter for videos uploaded in the calendar year 2024:</p>

```python
# Create new dataframe, to filter for videos from 2024, and for columns for analysis
New_df=video_df[['title', 'Publication Date', 'viewCount', 'likeCount', 'commentCount']]
New_df=New_df[(New_df['Publication Date'] > '2023-12-31') & (New_df['Publication Date'] < '2025-01-01')]
New_df['Publication Date'] = pd.to_datetime(New_df['Publication Date'])
New_df['Month'] = New_df['Publication Date'].dt.strftime('%B')
New_df.set_index('Month')
New_df
```

![image](https://github.com/user-attachments/assets/74064955-824d-4e72-be86-004ddc059214)

#### Step 5: Group by Months to Perform Month-by-Month Operations

<p>Use this code snippet to perform a groupby operation on the dataframe by month:</p>

```python
# Group by months
New_df = New_df.set_index(pd.to_datetime(New_df['Publication Date']))
grouped_df = New_df.groupby(pd.Grouper(freq='M'))
grouped_df
```

<p>Then we can find the monthly sum of each metric:</p>

```python
# Monthly sum of each metric
grouped_df[['viewCount', 'likeCount', 'commentCount']].sum()
Sum_df=pd.DataFrame(grouped_df[['viewCount', 'likeCount', 'commentCount']].sum())
Sum_df['Month'] = Sum_df.index.strftime('%B')
Sum_df.set_index('Month')

![image](https://github.com/user-attachments/assets/8c37dc8a-0a1b-4ba5-b0f1-50f89badad2e)

<p>The monthly average for each metric:</p>

```python
# Monthly average of each metric

grouped_df[['viewCount', 'likeCount', 'commentCount']].mean()
Mean_df=pd.DataFrame(grouped_df[['viewCount', 'likeCount', 'commentCount']].mean())
Mean_df['Month'] = Size_df.index.strftime('%B')
Mean_df.set_index('Month')
```

![image](https://github.com/user-attachments/assets/9309310c-5d6e-47f1-b800-a7df26e9a8ff)

<p>A similar operation can be executed for total uploads:</p>

```python
# Total number of video uploads by month

grouped_df.size()
Size_df=pd.DataFrame(grouped_df.size())
Size_df.rename(columns={0: 'Number of Uploads'}, inplace=True)
Size_df['Month'] = Size_df.index.strftime('%B')
Size_df.set_index('Month')
```

![image](https://github.com/user-attachments/assets/f306cc94-da69-46e6-b17b-0d4c4999638d)

<p>An extra operation for further analysis:</p>

```python
# Monthly per-video count

PerVid_df=pd.DataFrame([Sum_df['Month'], Sum_df['viewCount']/Size_df['Number of Uploads'],
                        Sum_df['likeCount']/Size_df['Number of Uploads'],
                        Sum_df['commentCount']/Size_df['Number of Uploads']]).transpose()
PerVid_df.columns=['Month', 'Views/Upload', 'Likes/Upload', 'Comments/Upload']
PerVid_df['Month'] = Size_df.index.strftime('%B')
PerVid_df.set_index('Month')
```

![image](https://github.com/user-attachments/assets/2dfc6d25-37a2-483e-b804-2a5aab8f532a)

#### Step 6: Top 5 for Each Metric for 2024

<p>Here is the code snippet for ascertaining the 5 most viewed videos of 2024, with English translation:</p>

```python
# 5 most viewed videos of 2024

Top5View_df=New_df[['title', 'viewCount']].nlargest(5, 'viewCount')
Top5View_df

# Translate titles to English
EnglishView=['Pinoy Super Visa holder passes away after suffering a stroke on a flight going to Canada',
             'Warning for temporary residents crossing the land border for processing in Canada',
             'Tourist hospitalized in Canada appeals for community help',
             'Beloved Pinoy nanny that a family has been looking for for a long time has been found',
             'Many immigrants are having difficulty paying bills']
Top5View_df['English Title']=EnglishView
Top5View_df
```

![image](https://github.com/user-attachments/assets/a80f34b7-757f-451d-bdf5-4d710ef5f580)

<p>You can use similar snippets for ascertaining the 5 most liked and 5 most commented videos, respectively. Just ensure to change the variables and column assignment:</p>

![image](https://github.com/user-attachments/assets/c58454e0-e5c2-485b-829b-fe3cfdf2bcef)

![image](https://github.com/user-attachments/assets/17b87b2c-3f07-4ca7-831b-636739be5399)

<p>For fun, here are the most viewed videos for each month:</p>

```python
# Most viewed video by month

MostView_df = grouped_df.apply(lambda x: x[['title', 'viewCount']].nlargest(1, 'viewCount'))
MostView_df
```

![image](https://github.com/user-attachments/assets/c0388b5d-cb72-44f1-94d4-9f9e5afd0eff)

</details>

<details><summary><h2>Part 3: Data Visualization with HV Plot</h2></summary> 

#### Step 1: Install Necessary Libraries

<p>hvplot and jupyter_bokeh will be used for interactivity. Import panel.</p>

<p>For generating a wordcloud, PIL and wordCloud will be used (specifically Image, and WordCloud, STOPWORDS, ImageColorGenerator)</p>

#### Step 2: Create First Panel Visualization (Monthly Statistics)

<p>Use the following snippet to generate an hv bar plot for total uploads per month:</p>

```python
## Panel 1: Monthly distribution of views, likes, comments, and uploads

# Change the column name from '0' to 'size'

Size_df['Month'] = Size_df.index.strftime('%B')
Size_df.set_index('Month')


fig1=Size_df.hvplot.bar(x='Month', y='Number of Uploads')
fig1.opts(title='Number of Video Uploads per Month', yformatter=formatter)
```
<p>Similarly, generate similar plots for monthly total likes, views, and comments, as well as their averages:</p>

```python
fig2=Sum_df.hvplot.bar(x='Month', y='viewCount')
fig2.opts(title='Total Views per Month',yformatter=formatter)

fig3=Sum_df.hvplot.bar(x='Month', y='likeCount')
fig3.opts(title='Total Likes per Month',yformatter=formatter)

fig4=Sum_df.hvplot.bar(x='Month', y='commentCount')
fig4.opts(title='Total Comments per Month',yformatter=formatter)
```
```python
fig2=Sum_df.hvplot.bar(x='Month', y='viewCount')
fig2.opts(title='Total Views per Month',yformatter=formatter)

fig3=Sum_df.hvplot.bar(x='Month', y='likeCount')
fig3.opts(title='Total Likes per Month',yformatter=formatter)

fig4=Sum_df.hvplot.bar(x='Month', y='commentCount')
fig4.opts(title='Total Comments per Month',yformatter=formatter)
```

<p>This is now where the panel library shines, because it amalgamates all of these plots into one interactive panel:</p>

```python
# Amalgamate bar plots
bars=pn.Tabs(("Uploads Per Month",fig1), ("Total Views Per Month",fig2),
            ("Total Likes Per Month", fig3), ("Total Comments Per Month",fig4),
            ("Avg Views Per Month",fig5), ("Avg Likes Per Month",fig6),
             ("Avg Comments Per Month", fig7))
bars
```

![image](https://github.com/user-attachments/assets/8321fe5c-bcd4-4cc6-94a7-c976514abd7a)

![image](https://github.com/user-attachments/assets/3e8f8e0d-a465-441e-aee5-b8d6bf588cc6)

![image](https://github.com/user-attachments/assets/996c5183-34bf-45d6-a599-99e0c9ac44af)

![image](https://github.com/user-attachments/assets/21734e03-36c3-4c81-a2da-e6812cb707c9)

#### Step 3: Create Second Panel Visualization (Top 5/Top 10 Tables)

<p>In a similar vein, we can get the top 5/top 10 tables, and amalgamate them into one interactive panel:</p>

```python
## Panel 2: Tables of most viewed, liked, and commented videos

t1=Top5View_df.drop('title', axis=1)
t1.name='Top 5 Most Viewed Videos of 2024'
t2=Top5Like_df.drop('title', axis=1)
t2.name='Top 5 Most Liked Videos of 2024'
t3=Top5Comment_df.drop('title', axis=1)
t3.name='Top 5 Most Commented Videos of 2024'
t4=MostView_df.drop('title', axis=1)
t4.name='Most Viewed Videos of 2024 by Month'


tabs = pn.Tabs(t1, t2, t3,t4)
tabs
```

![image](https://github.com/user-attachments/assets/2cc4b1ef-838d-4bbb-ac1f-3cef6941a4e5)

#### Step 4: Histogram Visualization (Views)

```python
## Panel 3: Distribution of Views

View_df=New_df[['viewCount']]
HistPlot=View_df.hvplot.hist(bins=10, title='Distribution of Views')
HistPlot.opts(xformatter=formatter)
HistPlot
```

![image](https://github.com/user-attachments/assets/92fff153-20ac-42ab-b941-3ef057ef8cd0)

#### Step 5: Tagalog Language Natural Language Processing

<p>Generate a wordcloud to visualize the most commonly used words in the video titles:</p>

```python
## Panel 4: Tagalog Language NLP
text=New_df['title'].str.cat(sep=' ')

# Create and generate a word cloud image:
wordcloud = WordCloud().generate(text)

# Display the generated image:
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()
```

![image](https://github.com/user-attachments/assets/ff11a229-41ae-4636-935d-915045c1ffc9)

<p>There are many words that are not useful to this analysis such as "OMNI". Geographic names such as cities, common Tagalog words such as "ang" ("the") are also not useful.</p>

<p>Use STOPWORDS to exclude certain words that are not useful to the analysis such as the ones that fall under either of the three categories. Then, generate a new wordcloud.</p>

```python
# Set stopwords to exclude geographic places, OMNI, words relating to "Filipino","Pinoy", and "Canadian",
# and the most common Tagalog words such as "ang" ("the")
stopwords = set(STOPWORDS)
stopwords.update(["OMNI News", "Filipino", "News", "Canada","Canadian", "Pilipino",
                  "Pinoy", "Philippines", "sa", "hindi","Pilipina", "ni", "nang", "para","parang", "OMNI", "sa mga",
                  "ang", "mga", "yung", "ng","may", "Pinay","na","isang","ngayong","ngayon",
                  "bilang","ilang","bagong","dating","Pilipinas","Canadians","BC",
                  "Alberta", "Saskatchewan","Manitoba","Ontario","Vancouver","Toronto","mag",
                  "Filipina","maging","mula","Philippine","dahil","Surrey","Calgary"])

# Generate a word cloud image
wordcloud = WordCloud(stopwords=stopwords, background_color="white").generate(text)

# Display the generated image:
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()
```

![image](https://github.com/user-attachments/assets/cc5f10e4-3846-4254-a106-58e2ec0efc5f)

</details>
