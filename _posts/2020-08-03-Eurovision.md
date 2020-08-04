---
layout: post
title: Eurovis(ualizat)ion
---

<img src="https://m.media-amazon.com/images/M/MV5BYzRjYzA5NTQtOTE3MC00OTYzLWEzODItMzQxYWE1NDJkMDA0XkEyXkFqcGdeQXVyMTkxNjUyNQ@@._V1_SY1000_CR0,0,675,1000_AL_.jpg" alt="Eurovision Song Contest" style="width:66%">

## Introduction

I recently watched Netflix's *Eurovision Song Contest: the Story of Fire Saga*. Starring Will Ferell and Rachel McAdams, this satirical take on the iconic song contest was, hands down, the second best thing that's happened to me this year after completing the Master of Data Science program. And I'm not even a huge fan of the actual competition!

Before I go any further, I should say that there won't be any spoilers here. I should also explain what the Eurovision Song Contest is for those who are unfamiliar with it. It's an annual televised competition that dates back to 1956, when it began as a way to bring Europe together through music. Although its format and participants have changed over the years, the gist of the competition remains more or less the same. Simply put, each participating country does two things: (1) sends a performer to represent their country and (2) ranks the other performances, with the help of a professional jury and televoting, to determine the winner. If you want to learn more about the rules of the contest, [this article](https://eurovision.tv/about/how-it-works) might be a good place to start.

Although we rarely talk about this event in North America, people take it seriously on the other side of the Atlantic Ocean. In 2019, 182 million viewers tuned into 41 countries battle it out through song and dance. To put that number into perspective, the most recent Super Bowl drew just under 100 million viewers.

With a rich history and avid fanbase, many people, including [this virologist at Lancaster University](https://www.bbc.com/news/av/entertainment-arts-22560481), have decided to analyze data derived from the competition. After watching the movie, not only did I have *Double Trouble* stuck in my head, I also had an urge to hop on the bandwagon and explore the data myself. I stumbled upon a few articles that tried to reveal patterns in voting behaviour and after reading them, I was inspired to use unsupervised learning to identify which countries vote similarly.

## Dataset description

Luckily, I found a [dataset](https://data.world/datagraver/eurovision-song-contest-scores-1975-2019) that contained all scores given from 1975 until 2019 during the finals and semi-finals. I only included observations since the start of the millennium since there was a surge of countries that joined the competition around that time. I also want to acknowledge that Eurasia has seen many geopolitical changes during the last 100 years. For that reason, I only kept observations belonging to nations that currently exist so that the voting behaviour could eventually be visualized using the world map supported by Altair. I should also mention that, while each country gives two sets of points, I only considered the jury vote at the finals for this analysis since televoting results weren't consistently available over the years. If you're curious, the video below provides a thorough explanation of the current voting procedure.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Wd_RHS3f5-4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Data wrangling

I began by cleaning the data and creating a data frame in which the rows held the points *given* by a country and the columns held the points that a country *received* during a specific year. A snippet of the data frame is shown below. By organizing the data in this way, each row can be seen as a vector that holds information about a particular country's musical and artistic preferences. Countries that have voted similarly throughout the contest's history should be represented by vectors that are close to each other. By extension, we can infer that they have similar preferences.

![]({{ site.baseurl }}/imgs/eurovision_data_nan.PNG)

There was a little cleaning up left to do before I could do any modeling. I had to make sure that there were no missing values in the data frame. Since countries can't give points to themselves, scores were missing where the row and column referred to the same country. For example, in the data frame above, scores for `Albania2004` through `Albania2019` are missing for `Albania`. In these situations, I imputed the missing values with 12. I assumed that, if they could, each country would give themselves the highest score possible (12 points) since their performance should be something that perfectly captures their interest and preference. Missing values were attributable to another reason: if a country didn't participate in the contest in a given year, they wouldn't have been able to give points to the other countries. In these cases, each missing score was replaced with 0 since that's the most probable score that a participating country would give to another country (i.e. the mode).

Once the data frame were free of missing values, we were ready for hierarchical clustering.

![](https://media.giphy.com/media/jpbi38x5UZZYxKq35c/giphy.gif)

## Hierarchical clustering

I played around with a few different clustering techniques and distance measures. I ended up using the Ward technique with Euclidean distance, merging clusters based on what results in the smallest increase in inertia (i.e. within-cluster dissimilarity). The dendogram is shown below.

![]({{ site.baseurl }}/imgs/eurovision_dendogram.PNG)

It looks like there are five distinct groups. Here were some of my initial observations:

- There's an odd cluster consisting of Australia (yes, you read that right) and microstates like Andorra, Monaco, and San Marino.
- Does the proximity of Spain and Portugal to Romania have anything to do with the fact that they belong to the same language family?

## Visualizing the voting preferences

From there, I used scikit-learn's [Agglomerative Clustering](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html) to retrieve the cluster labels. Then, I visualized the labels on a choropleth map using Altair. Countries in dark blue either don't have data or aren't participants of the Eurovision Song Contest.

![]({{ site.baseurl }}/imgs/eurovision_choropleth.PNG)

This map suggests that Nordic and Baltic countries in addition to the British Isles (shown in red) vote similarly to one another. It also appears that the French- and German-speaking countries in Western Europe (shown in yellow) have similar preferences. Interestingly, while Balkan nations (shown in light blue) belong to one voting bloc, there is another cluster made up of former members of the Eastern Bloc (shown in orange). In general, these observations are in line with findings from similar analyses. 

## Wrapping up

In the future, I'd like to incorporate the results from televoting. Although each country's jury panel is supposed to be demographically representative of that country, it'd still be interesting to see how the clustering would change if the people's votes are included in the analysis.

Well, you've made it to the end of the post! For your listening pleasure, I've included the Spotify playlist containing the songs that would've competed at this year's contest if it wasn't cancelled due to the pandemic. Iceland's *real* entry *Think About Things* is a real bop.

<iframe src="https://open.spotify.com/embed/playlist/0xGk0xR6fI88NsmiHZZMPE" width="300" height="380" frameborder="0" allowtransparency="true" allow="encrypted-media"></iframe>

The source code can be found [here](https://github.com/reikookamoto/eurovision/blob/master/src/analysis.ipynb).

### Attribution
1. [182 million viewers tuned in to the 2019 Eurovision Song Contest](https://eurovision.tv/story/182-million-viewers-2019-eurovision-song-contest)
2. [Super Bowl 2020 scores 99.9 million TV viewers with Chiefs comeback](https://www.latimes.com/entertainment-arts/business/story/2020-02-03/super-bowl-2020-scores-99-9-million-tv-viewers-with-chiefs-comeback)
3. [What's behind the Eurovision voting](https://medium.com/yottabytes/what-s-behind-the-eurovision-voting-3e154d42fb47)
4. [The Eurovision Song Contest Analyzed](http://37steps.com/4536/eu-song-contest/)
