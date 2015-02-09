---
layout: post
title: "Is Fantasy Basketball an Excuse to Learn Basic Statistics?"
date: 2015-02-09T15:54:23-05:00
---

# Introduction

The purpose of this series of posts is to explore basic statistics and introductory statistcal programming using  R in the context of a fantasy basketball league. In this blog post I'll give an overview of the league I play in, cover how I acquired the data to analyze, and show how simple descriptive statistics can explain relationships between scoring categories. 

## The League

I play in a [fantasy basketball league](http://basketball.fantasysports.yahoo.com/nba/177177). The league operates in a head-to-head format, with two teams of players squaring off each weak. The winner is determined based on who's team "wins" the majority of the following nine categories.

### My Fantasy League's Scoring Categories

1. Field Goal Percentage (FG.)
2. Free Throw Percentage (FT.)
3. Three Point Shots Made (X3P)
4. Total Points Scored (PTS)
5. Total Rebounds (TRB)
6. Total Assists (AST)
7. Total Steals (STL)
8. Total Blocks (BLK)
9. Total Turnovers (TOV). 

In each of the categories, except for turnovers, a higher number is better, and the person who 'wins' five or more of the categories wins that week's game. Winning each category counts as one point, whether you have 1 or 1,000 more steals (or blocks, or assists) than your opponent. Futhermore, several of these categories are obviously related, for instance, Total Points Scored must be at least three times greater than Three Point Shots Made, and players who make lots of three tend to score lots of points. This seems (and is) obvious, but let's see what sort of more complicated relationships we can discover with *data*.

    
# Data Acquisition

For this post I decided to use statistics from the last complete (2013-2014) basketball season. I gathered the statistics from [basketball reference](http://www.basketball-reference.com/). I exported the statiscs from [this](http://www.basketball-reference.com/leagues/NBA_2014_totals.html) webpage as a .csv file.

First I load the .csv file into R, and use the `dim()` command to see the size of the data:

{% highlight R %}

RawBballData<-read.csv("~/BasketballData/leagues_NBA_2014_totals_totals.csv",stringsAsFactors=FALSE)

dim(RawBballData)

	[1] 506  30


{% endhighlight %} 

[`dim()`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/dim.html) outputs two numbers, first the rows, and then the columns of a matrix, array, or data.frame (some of R's [datatypes](http://adv-r.had.co.nz/Data-structures.html))

So we can see that we are working with dataset that contains 635 rows and 30 columns. Each row represents a unique player, and each column represents a particular statistic. We only care about the 9 statistics that correspond to scoring categories in my league and the player's name. Let's extract the variables of interest. Note: I do some behind the scenes data manipulation to deal with players who played for multiple teams, the code for this [manipulation](https://github.com/) is documented in full on my github.

{% highlight R %}

names(RawBballData)

> names(RawBballData)
 [1] "Rk"     "Player" "Pos"    "Age"    "Tm"     "G"      "GS"     "MP"     "FG"     "FGA"    "FG."    "X3P"   
[13] "X3PA"   "X3P.1"  "X2P"    "X2PA"   "X2P.1"  "eFG."   "FT"     "FTA"    "FT."    "ORB"    "DRB"    "TRB"   
[25] "AST"    "STL"    "BLK"    "TOV"    "PF"     "PTS"   

scoringCategories<-c("Player","PTS","TOV","BLK","STL","AST","TRB","FT.","FG.","X3P")

BballData<-RawBballData[scoringCategories]

{% endhighlight %} 

We can see what the first lines of our data look like by using the `head()` command. 

{% highlight R %}

head(BballData)


{% endhighlight %}

   |  Player | PTS | TOV | BLK | STL | AST | TRB | FT. | FG. | X3P |
   |---------|----:|----:|----:|----:|----:|----:|----:|----:|----:|
   | A.J. Price | 44 | 7 | 0 | 1 | 13 |  10 | .000 | .413 | 6 |
   |Aaron Brooks | 645 | 117 | 13 | 52 | 233 | 140 | .874 | .401 | 96 |
   | Aaron Gray | 65 | 31  | 8 |  10 | 22 | 111 | .550 |.443 | 0
 |Adonis Thomas | 14 |  1  | 0 |  0 | 3 | 3 | 1.000 | .429 | 1
 |Al Harrington | 225 | 34 | 0 |  14 | 28 | 80 | .771 | .396 | 34
  |  Al Horford | 538 | 64 | 44 | 27 | 76 | 244 |.682 | .567 |4


A.J. Price, Adonis Thomis, and Al Harrington all had as many blocks as I did! Maybe someday I'll be an NBA player too. 


<figure>
	<img src="/images/BlogPic.png">
	<figcaption>NBA Stat Correlations</figcaption>

</figure>
