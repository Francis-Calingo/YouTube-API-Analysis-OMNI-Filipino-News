# PROJECT OVERVIEW: YouTube API Analysis, OMNI Filipino News' 2024 Coverage (with Python)
  <ul>
    <li>Used YouTube's API to extract data from OMNI Television's Filipino playlist (particularly its 2024 videos).</li>
      <li>OMNI, one of the largest multilingual media outlets in Canada, offers services in Tagalog ("Filipino"). Therefore, scraping data on their Filipino playlist to ascertain and discern potentially interesting insights on topics that mattered a lot to the Filipino Canadian community.</li>
    <li>Used Pandas, hvplot, and natural language processing to visualize the data.</li>

  </ul>
  
## Code and Resources Used
  <ul>
    <li><b>IDEs Used:</b> Google Colab, Jupyter Notebook</li>
    <li><b>Python Version:</b> 3.10.12</li>
    <li><b>Libraries and Packages:</b>
    <ul>
      <li><b>API Libraries: </b> geopandas, matplotlib.plt </li>
      <li><b>Libraries for data manipulation and visualization: </b> pandas, seaborn, plotly.express, numpy, hvplot</li>
      <li><b>Libraries for natural language processing: </b> pandas, seaborn, plotly.express, numpy</li>
    </ul></li>
  </ul>

## YouTube API Introduction
<p>An application program interface (API) is a mechanism that allows for two computer applications to communicate and connect with each other. Many websites, from the National Hockey League to YouTube, have their own API, which allows local machines such as your laptop and mobile phone to access them and their data. In fact, for this project, YouTube API will be used, as it is a helpful tool in ascertaining a wide variety of data from YouTube channels, from its engagements to upload counts to qualitative data such as its most-liked comments. However, before beginning any project involving YouTube's API, one will need an API key. Instructions to receive the key are stipulated on its website: https://developers.google.com/youtube/v3/getting-started . A Google account is needed. Then, set a variable equal to the key for convenience. Keys are uniquely generated, so it will be different for each user.</p>

![image](https://github.com/user-attachments/assets/62df7c64-64ad-4dd0-8d9e-9605a7128f83)

## Part 1: Scraping Channel and Video Data Using YouTube API

#### Step 1: Get channel's ID

<p>To get any channel's ID, you'll have to click "share channel" in the "About" section, then you will see a button that will say "copy channel ID".</p>

![image](https://github.com/user-attachments/assets/b578f53e-94fe-4f29-b6fc-9695c3f9a4d1)

<p>After copying it, paste it in your notebook and set is as a variable for convenience.</p>

![image](https://github.com/user-attachments/assets/dd62c53e-f5b8-477c-a12e-66686c02fb30)

#### Step 2: Get channel data

<p>Use the following two codes in sequential order in order to scrape the channel's data using the channel ID. Much of the code snippets are actually found in the YouTube API website (https://developers.google.com/youtube/v3/docs/channels/list):</p>

![image](https://github.com/user-attachments/assets/90f8d978-aa18-46a4-bc17-4b55da59f565)

![image](https://github.com/user-attachments/assets/61374433-112d-4a4a-9440-02d11e053b88)

<p>We now have our channel data:</p>

![image](https://github.com/user-attachments/assets/ca71405b-ee29-47db-bf2e-8adb1ecfef0a)

#### Step 3: Get playlist ID

<p>To get the playlist ID, go to the playlist of interest, click "share", and you will see its url. The ID is located in the url, after "list=": </p>

![image](https://github.com/user-attachments/assets/b369c794-0919-4589-bfd6-bc6e28c3d364)

<p>Then use the following code snippet (taken from https://developers.google.com/youtube/v3/docs/playlists/list):</p>

![image](https://github.com/user-attachments/assets/cbe5fabb-efec-485d-b724-503c2fcf39c6)

#### Step 4: Ascertain Video Details of the playlist

<p>First, set the playlist ID as a variable for convenience:</p>

![image](https://github.com/user-attachments/assets/ed116384-8cc0-43e1-9c7c-7018e1197953)

<p>Then use the following code snippet that partially draws from this webpage (https://developers.google.com/youtube/v3/docs/videos/list): </p>

![image](https://github.com/user-attachments/assets/41c9b513-bdb3-4c00-b419-329418581ded)

<p>To check if the function works, check if the total number of entries ("length") matches the total number of videos uploaded, which it indeed does (as of the completion date of this project): </p>

![image](https://github.com/user-attachments/assets/7334e79f-8b5a-4347-9501-b8a48556dfac)

<p>Now we can use this code snippet to get video statistics, partially drawing upon this webpage (https://developers.google.com/youtube/v3/docs/videos/list): </p>

![image](https://github.com/user-attachments/assets/58981a22-d9e7-4301-89b3-14742cc9a905)

<p>The result: </p>

![image](https://github.com/user-attachments/assets/588b8783-e50d-463e-8947-89b2f3c0ce04)

## Part 2: Data Cleaning and Pre-Processing Using Pandas

#### Step 1: Pandas Operations for "Likes", "Comments", "Views"

<p>The three mentioned variables constitute a video's engagement. We perform a check to ensure that there are no null values, as that could cause issues.</p>

<p>Then, we convert those three columns into "numeric" columns, as in the dataframe, even though the counts obviously represent numerical values, it is currently stored as "strings" (or, treated as "words").</p>

#### Step 2: Ensure Publication Dates are Stored as Dates

<p>In a similar vein, we want the dates to be stored as dates, not as strings. Therefore, this pandas operation was executed to convert their datatype:</p>

![image](https://github.com/user-attachments/assets/716e5369-e984-4682-8573-492f7eab3ecc)

<p>The column can then be added to the dataframe.</p>

#### Step 3: Drop Duplicated Titles

<p>Duplicated entries in the "title" column represents duplicate uploads and are not useful for this analysis. Check for any duplicates in that columns, then drop their associated rows.</p>

#### Step 4: Filter for 2024 Videos

<p>Use this code snippet to filter for videos uploaded in the calendar year 2024:</p>

![image](https://github.com/user-attachments/assets/74064955-824d-4e72-be86-004ddc059214)

#### Step 5: Group by Months to Perform Month-by-Month Operations

<p>Use this code snippet to perform a groupby operation on the dataframe by month:</p>

![image](https://github.com/user-attachments/assets/a74b4df4-7bc2-4f31-bca7-009d7b60a8dc)

<p>Then we can find the monthly sum of each metric:</p>

![image](https://github.com/user-attachments/assets/8c37dc8a-0a1b-4ba5-b0f1-50f89badad2e)

<p>The monthly average for each metric:</p>

![image](https://github.com/user-attachments/assets/9309310c-5d6e-47f1-b800-a7df26e9a8ff)

<p>A similar operation can be executed for total uploads:</p>

![image](https://github.com/user-attachments/assets/f306cc94-da69-46e6-b17b-0d4c4999638d)

<p>An extra operation for further analysis:</p>

![image](https://github.com/user-attachments/assets/2dfc6d25-37a2-483e-b804-2a5aab8f532a)

#### Step 6: Top 5 for Each Metric for 2024

<p>Here is the code snippet for ascertaining the 5 most viewed videos of 2024, with English translation:</p>

![image](https://github.com/user-attachments/assets/a80f34b7-757f-451d-bdf5-4d710ef5f580)

<p>You can use similar snippets for ascertaining the 5 most liked and 5 most commented videos, respectively. Just ensure to change the variables and column assignment:</p>

![image](https://github.com/user-attachments/assets/c58454e0-e5c2-485b-829b-fe3cfdf2bcef)

![image](https://github.com/user-attachments/assets/17b87b2c-3f07-4ca7-831b-636739be5399)

<p>For fun, here are the most viewed videos for each month:</p>

![image](https://github.com/user-attachments/assets/c0388b5d-cb72-44f1-94d4-9f9e5afd0eff)

## Part 3: Data Visualization with HV Plot

#### Step 1: Install Necessary Libraries

<p>hvplot and jupyter_bokeh will be used for interactivity. Import panel.</p>

<p>For generating a wordcloud, PIL and wordCloud will be used (specifically Image, and WordCloud, STOPWORDS, ImageColorGenerator)</p>

#### Step 2: Create First Panel Visualization (Monthly Statistics)

<p>Use the following snippet to generate an hv bar plot for total uploads per month:</p>

![image](https://github.com/user-attachments/assets/fb415cb7-b519-4aa8-bde3-d2399f2b42d5)

<p>Similarly, generate similar plots for monthly total likes, views, and comments, as well as their averages:</p>

![image](https://github.com/user-attachments/assets/ca1d8de9-6d7c-478b-8106-dd69f5652999)

![image](https://github.com/user-attachments/assets/06ec473e-2361-4c65-b784-e24e404ef225)

<p>This is now where the panel library shines, because it amalgamates all of these plots into one interactive panel:</p>

![image](https://github.com/user-attachments/assets/8321fe5c-bcd4-4cc6-94a7-c976514abd7a)

![image](https://github.com/user-attachments/assets/3e8f8e0d-a465-441e-aee5-b8d6bf588cc6)

![image](https://github.com/user-attachments/assets/996c5183-34bf-45d6-a599-99e0c9ac44af)

![image](https://github.com/user-attachments/assets/21734e03-36c3-4c81-a2da-e6812cb707c9)

#### Step 3: Create Second Panel Visualization (Top 5/Top 10 Tables)

<p>In a similar vein, we can get the top 5/top 10 tables, and amalgamate them into one interactive panel:</p>

![image](https://github.com/user-attachments/assets/2cc4b1ef-838d-4bbb-ac1f-3cef6941a4e5)

#### Step 4: Histogram Visualization (Views)

![image](https://github.com/user-attachments/assets/92fff153-20ac-42ab-b941-3ef057ef8cd0)

#### Step 5: Tagalog Language Natural Language Processing

<p>Generate a wordcloud to visualize the most commonly used words in the video titles:</p>

![image](https://github.com/user-attachments/assets/ff11a229-41ae-4636-935d-915045c1ffc9)

<p>There are many words that are not useful to this analysis such as "OMNI". Geographic names such as cities, common Tagalog words such as "ang" ("the") are also not useful.</p>

<p>Use STOPWORDS to exclude certain words that are not useful to the analysis such as the ones that fall under either of the three categories. Then, generate a new wordcloud.</p>

![image](https://github.com/user-attachments/assets/cc5f10e4-3846-4254-a106-58e2ec0efc5f)

## Discussion

<p>The resulting data visualizations show that conditions of immigrants (especially in the context of international students and recent changes to Canadian immigration law),
and Philippine politics in relation to former president Rodrigo Duterte were the most covered and discussed topics in the community in 2024.
There was a spike of video uploads during the summer months, especially in June (Philippine Heritage Month) and August (Taste of Manila Festival in Toronto).
Regardless, much of the engagements and discussions still stemmed from the two aforementioned topics.
With the upcoming Philippine and Canadian general elections, it is of utmost importance for candidates to address the Filipino Canadian community’s concerns with immigration policies and their effects on them.
</p>


