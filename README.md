# PROJECT OVERVIEW: YouTube API Analysis, OMNI Filipino News' 2024 Coverage (with Python)

## Table of Contents
* [Introduction](#introduction)
* [Code and Setup](#code-and-setup)
* [YouTube API Introduction](#youtube-api-introduction)
* [Results Summary: Scraping Channel and Video Data Using YouTube API](#results-summary-scraping-channel-and-video-data-using-youtube-api)
* [Results Summary: Data Cleaning and Pre-Processing Using Pandas](#results-summary-data-cleaning-and-pre-processing-using-pandas)
* [Results Summary: Data Visualization with HV Plot](#results-summary-data-visualization-with-hv-plot)
* [Discussion](#discussion)
* [Recommendations](#recommendations)
* [Assumptions and Caveats](#assumptions-and-caveats)
* [Credits and Acknowledgements](#credits-and-acknowledgements)

<details><summary><h2>Introduction</h2></summary> 
  <ul>
    <li>Used YouTube's API to extract data from OMNI Television's Filipino playlist (particularly its 2024 videos).</li>
    <li>OMNI, one of the largest multilingual media outlets in Canada, offers services in Tagalog ("Filipino"). Therefore, scraping data on their Filipino playlist to ascertain and discern potentially interesting insights on topics that mattered a lot to the Filipino Canadian community.</li>
    <li>Used Pandas, hvplot, and natural language processing to visualize the data.</li>

  </ul>

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>
  
<details><summary><h2>Code and Setup</h2></summary> 
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

If you'd like to fork or run this locally:

```bash
git clone https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News.git
cd YouTube-API-Analysis-OMNI-Filipino-News
```

To install Python requirements:
```bash
pip install -r requirements.txt
```

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>YouTube API Introduction</h2></summary> 

<p>An application program interface (API) is a mechanism that allows for two computer applications to communicate and connect with each other. Many websites, from the National Hockey League to YouTube, have their own API, which allows local machines such as your laptop and mobile phone to access them and their data. In fact, for this project, YouTube API will be used, as it is a helpful tool in ascertaining a wide variety of data from YouTube channels, from its engagements to upload counts to qualitative data such as its most-liked comments. However, before beginning any project involving YouTube's API, one will need an API key. Instructions to receive the key are stipulated on its website: https://developers.google.com/youtube/v3/getting-started . A Google account is needed. Then, set a variable equal to the key for convenience. Keys are uniquely generated, so it will be different for each user.</p>

```python
# Get API key, see https://developers.google.com/youtube/v3/getting-started
api_key='AIzaSyD0yX1_guVPsz-Aol324Bl3aUFlh86b-hM'
```

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>Results Summary: Scraping Channel and Video Data Using YouTube API</h2></summary> 

#### Channel data

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure1.2.png"/>

#### Video Details of the Filipino playlist

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure1.4.png"/>

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>Results Summary: Data Cleaning and Pre-Processing Using Pandas</h2></summary> 


#### Filter for 2024 Videos

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.1.png"/>

#### Group by Months

<p>The monthly average for each metric:</p>

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.2.png"/>

<p>Monthly total uploads:</p>

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.3.png"/>

<p>An extra operation for further analysis:</p>

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.4.png"/>

#### Top 5 for Each Metric for 2024

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.5.png"/>

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.6.png"/>

#### Most Viewed Videos for Each Month

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure2.7.png"/>

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>Results Summary: Data Visualization with HV Plot</h2></summary> 


#### Monthly Statistics

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure3.1.png"/>

#### Top 5/Top 10 Tables

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure3.2.png"/>


#### Histogram Visualization (Views)

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure3.3.png"/>


#### Tagalog Language WordCloud of Titles (with STOPWORD)

<p>Generate a wordcloud to visualize the most commonly used words in the video titles (excluding  words from a custom STOPWORD list which blocks of certain words such as common words and geographical areas):</p>

<img src="https://github.com/Francis-Calingo/YouTube-API-Analysis-OMNI-Filipino-News/blob/main/Figures/Figure3.5.png"/>

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>Discussion</h2></summary> 

* **Uploads peaked in the summer months, but total views, comments, and likes peaked near the end of 2024.** The higher upload amount in the summer may reflect the fact that the higher presence of community events in the late spring/summer months (especially in June, which is observed as Philippine Heritage Month in Canada) drives up OMNI News Filipino Edition's coverage capabilities, but the higher total engagements in the latter part of 2024 suggests that other topics are of greater importance and interest to the online Filipino Canadian community.
  
* **The Top 5 Most Viewed videos of 2024 all covered stories involving the conditions of the Filipino diaspora as a whole or at an individual level.** This suggests that migration topics remain of high interest to the Filipino diaspora.

* **The vast majority of videos garnered less than 20,000 views, with a select few garnering over 100,000.** In other words, OMNI Filipino coverage exceeding 20,000 views on YouTube would be considered viral.

* **Based on the WordCloud, it appears that words pertaining to cultural events (e.g., "festival", "Heritage Month") or migration (e.g., "caregiver", "international student") were the most frequently mentioned words amongst OMNI News Filipino's 2024 YouTube titles.** It appears that cultural events and migrations stories recieve the most coverage from OMNI News Filipino.

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>Recommendations</h2></summary> 

<p>The findings may, at first, not much actionable insights from a public policy lens. Regardless, the findings can be helpful for research purposes, especially research in social science, demography, and ethnic media.</p>

* The increase in uploads during the summer did not translate over for engagements, which peaked around the end of the year. This suggests that the Filipino Canadian online community following OMNI News Filipino's coverage are more inclined towards more specific topics, which is highly possible given that more specific prominent events that occurred at the latter part of the year such as changes to the Canadian immigration system and political events and mobilizations in support of former Philippine President Rodrigo Duterte garnered high levels of engagements. That is something for researchers whose focus area is the Filipino Canadian community to consider, albeit, should be careful not to conflate Filipino Canadian online engagement with broad Filipino Canadian concensus.

* Migration stories or stories of Philippine nationals in Canada recieved the most views from 2024. This reflects the importance of the topic of migration to the community. Organizations and institutions conducting community outreach to the community-be it governmental, educational, healthcare, business, etc.-should draft strategies for effectively reaching out to the Filipino Canadian community, especially only.

* OMNI News Filipino's YouTube videos typically garner less than 20,000 views. There are many possible factors that could be the cause of this, so it would be difficult to ascertain or even hypothesize them. Regardless, this presents an opportunity to study media consumption of the Filipino Canadian community, and attempt to determine if the community as a whole is more inclined (whether by choice or due to accessibility reasons) to consume print media rather than digital media (and even for those that consume digital media, if they have a preference for English-language media).

* Stories covering cultural events and migration recieved the most coverage from OMNI News Filipino Edition. While covering such stories is important (and, as the plots show, migration stories remain deply important to the Filipino Canadian online community), it is important for the outlet to ensure that the outlet gives a fair amount of coverage to other topics that may also be just as pertinent to the Filipino Canadian community. Actions such as consistent availability and opportunities for community members to give feedback could help in that regard.

* The findings could be scaled up and productionized into an interactive online dashboard that OMNI News could utilize to engage with their viewers and grow their audience.

* OMNI News also has Italian, Punjabi, Arabic, Cantonese, and Mandarin editions. There is an opportunity to perform similar analyses on neach edition and attempt to ascertain online community engagement for their different language newscasts. This would be valuable, as it is possible that, given Canada's increasingly multicultural demographic, OMNI may seek to expand the number of languages they offer their newscasts in.

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

<details><summary><h2>Assumptions and Caveats</h2></summary> 
 
* Currently, there is a lack of NLP capabilities for Tagalog language. Which is why I had to manually create a STOPWORDS list based on my working knowledge of Tagalog, as well as the appearance of the WordCloud. Of course, that method presents limitations. Currently there are efforts to develop Tagalog NLP pipelines and libraries (for example, [calamanCY](https://github.com/ljvmiranda921/calamanCy), an attempt to develop a Tagalog NLP framework with spaCY), but they remain in their infancy and are lagrely untested.

* No sentiment analysis was performed on comments. The comments section is a mix of Tagalog and English, which would make any kind of analysis with current Python libraries (see above point). However, analyzing them would still be valuable.

* Engagement on OMNI News' Filipino YouTube channel may not be reflective of the wider online Filipino Canadian community, much less the Filipino Canadian community as a whole. More comprehensive analyses on other Tagalog media (digital and media) would be required.
  
</details>

<details><summary><h2>Credits and Acknowledgements</h2></summary> 

"Tabs — Panel v1.6.3." https://panel.holoviz.org/reference/layouts/Tabs.html

"Youtube API for Python: How to Create a Unique Data Portfolio Project". Uploaded by Thu Vu, 2022-01-22. YouTube, https://www.youtube.com/watch?v=D56_Cx36oGY

[<b>Back to Table of Contents</b>](#table-of-contents)
</details>

