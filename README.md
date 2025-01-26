# PROJECT OVERVIEW: YouTube API Analysis, OMNI Filipino News' 2024 Coverage (with Python)
  <ul>
    <li>Used YouTube's API to extract data from OMNI Television's Filipino playlist (particularly its 2024 videos).</li>
      <li>OMNI, one of the largest multilingual media outlets in Canada, offers services in Tagalog ("Filipino"). Therefore, scraping data on their Filipino playlist to ascertain and discern potentially interesting insights on topics that mattered a lot to the Filipino Canadian commuinity.</li>
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
<p>An application program interface (API) is a mechanism that allows for two computer applications to communicate and connect with each other. Many websites, from the National Hockey League to YouTube, have their own API, which allows local machines such as your laptop and mobile phone to access them and their data. In fact, for this project, YouTube API will be used, as it is a helpful tool in ascertaining a wide variety of data from YouTube channels, from its engagements to upload counts to qualitative data such as its most-liked comments. However, before beginning any project involving YouTube's API, one will need an API key. Instructions to recieve the key is stipulated on its website: https://developers.google.com/youtube/v3/getting-started . A Google account is needed. Then, set a variable equal to the key for convenience. Keys are uniquely generated, so it will be diffrerent for each user.</p>

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

<p>Now we can use this code snippet to get video statisticsz, partially drawing upon this webpage (https://developers.google.com/youtube/v3/docs/videos/list): </p>

![image](https://github.com/user-attachments/assets/58981a22-d9e7-4301-89b3-14742cc9a905)

<p>The result: </p>

![image](https://github.com/user-attachments/assets/588b8783-e50d-463e-8947-89b2f3c0ce04)

## Part 2: Data Cleaning and Pre-Processing Using Pandas

## Part 3: Data Visualization with HV Plot

## Discussion




