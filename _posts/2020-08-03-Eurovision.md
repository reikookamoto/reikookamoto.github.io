---
layout: post
title: Eurovis(ualizat)ion
---

<img src="https://m.media-amazon.com/images/M/MV5BYzRjYzA5NTQtOTE3MC00OTYzLWEzODItMzQxYWE1NDJkMDA0XkEyXkFqcGdeQXVyMTkxNjUyNQ@@._V1_SY1000_CR0,0,675,1000_AL_.jpg" alt="Eurovision Song Contest" style="width:40%">

I recently watched Netflix's *Eurovision Song Contest: the Story of Fire Saga*. Starring Will Ferell and Rachel McAdams, this satirical take on the iconic song contest was, hands down, the second best thing that's happened to me this year after completing the Master of Data Science program. And I'm not even a huge fan of the actual competition! I hadn't even heard of the Eurovision Song Contest until I studied abroad in Sweden during my undergrad. My mom visited me during the week of the grand final and I just remember watching the show on TV with her in utter confusion.

Before I go any further, I should say that there won't be any spoilers here. I should also explain what the Eurovision Song Contest is for those who are unfamiliar with it. It's an annual televised competition that dates back to 1956, when it began as a way to bring Europe together through music. Although the format and participants of the competition have changed over the years, the gist remains more or less the same. Simply put, each participating country does two things: (1) sends a performer to represent their country and (2) ranks the other performances, with the help of a professional jury and televoting, to determine the winner. If you want to learn more about the rules of the contest, [this article](https://eurovision.tv/about/how-it-works) might be a good place to start.

Although we rarely talk about this event in North America, people take it seriously on the other side of the Atlantic Ocean. In 2019, 182 million viewers tuned into 41 countries battle it out through song and dance. To put that number in perspective, the most recent Super Bowl drew just under 100 million people.

With a rich history and avid fanbase, many people, including [this virologist at Lancaster University](https://www.bbc.com/news/av/entertainment-arts-22560481), have decided to analyze data derived from the competition. After watching the movie, not only did I have *Double Trouble* stuck in my head, I also had an urge to hop on the bandwagon and explore Eurovision data myself. I stumbled upon a few articles that tried to identify trends in voting behaviour and after reading them, I was inspired to use unsupervised learning to identify which countries vote similarly. In addition, I wanted to examine whether there are any underlying reasons for these patterns.

Luckily, I found a dataset that contained all scores given from 1975 until 2019 during the finals and semi-finals. I only included observations since the start of the millennium because there was a surge of countries that joined the competition around that time. I also want to acknowledge that Eurasia has seen many geopolitical changes during the last 100 years. I only kept observations belonging to nations that currently exist so that the voting behaviour could eventually be visualized using the world map supported by Altair. I should also mention that, while each country gives two sets of points, I only included votes from the juries at the finals since televoting results weren't consistently available over the years. If you're curious, the video below provides a thorough explanation of the current voting procedure.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Wd_RHS3f5-4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I began by wrangling the data and creating a table in which the rows contained the points *given* by a country and the columns contained the points *received* by a country during a specific year. A snippet of the data frame is shown below. By organizing the data in this way, each row can be seen as a vector that holds information about a particular country's musical and artistic preferences. Countries that have voted similarly throughout the contest's history should be represented by vectors that are close to each other. By extension, we can also infer that they have similar preferences.



### Sources
1. https://eurovision.tv/story/182-million-viewers-2019-eurovision-song-contest
2. https://www.latimes.com/entertainment-arts/business/story/2020-02-03/super-bowl-2020-scores-99-9-million-tv-viewers-with-chiefs-comeback
3. https://medium.com/yottabytes/what-s-behind-the-eurovision-voting-3e154d42fb47
4. http://37steps.com/4536/eu-song-contest/
