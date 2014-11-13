---
layout: post
title: 'Schloss Lab Fantasy Football Update: Playoff Scenarios'
author: "Niel"
date: "November 12, 2014"
comments: true
output:
  html_document:
    keep_md: yes
---

![](/figs/mora.png)

We are approaching the last weekend of the regular season in the Schloss et al. Fantasy Football League (SFFL), which means playoffs are right around the corner.  Because I'm such a generous commissioner, 8 out of 10 teams will make the playoffs and have a chance of becoming the SFFL 2014 Champion.  

In this post I will cover  
- current league standings  
- who will and won't make the playoffs  
- how teams on the bubble can make it in  
- calculate the probability of those bubble teams making it in

###Current Standings
I know the standings are available on ESPN, but who would want to go to ESPN.com when they could go to my blog instead? 

Playoff Seed | Team | Owner | Wins | Losses | Points For
-------------|------|-------|------|--------|-----------
1 | Flossin' RARAs | Sarah B | 7 | 3 | 1145.5
2 | Free Lunch | Kathryn | 7 | 3 | 1098.5
3 | Team U-MICHed me! | Chris | 7 | 3 | 1086.5
4 | 20% effort 80% victory | Niel | 5 | 5 | 1102.5
5 | Team Didit4thelulz | Matt | 5 | 5 | 1032.5
6 | Team USA | Amanda | 5 | 5 | 995
7 | Tibbers Domination | Alyx | 4 | 6 | 1144.5
8 | Curmudgeonly SchlossBot | PatBot | 4 | 6 | 956.5
Out | Jhansi, def not Matt | Jhansi? | 4 | 6 | 898.5
Out | Team McCrone | JT | 2 | 8 | 820.5

###Playoff Scenarios for Bubble Teams
Teams currently ranked 1-6 have already clinched a spot in a playoff, so their games this weekend will only effect seeding for the playoff bracket.  On the other end of the spectrum, Team McCrone has zero chance of making the playoffs.  Had JT joined the Schloss lab, he may have acquired the computational skillz needed to compete at such a high level of Fantasy Football. Alas, he did not.  Better luck next year.

Where it gets really interesting is for the three teams with a 4-6 record.  By the end of this weekend, two will advance to the playoffs, and one will be stuck playing Team McCrone for three straight weeks of consolation matchups. To make it even more exciting, two of those teams are playing against each other (Curmudgeonly SchlossBot and Tibbers Domination).  Here are the scenarios where each of those three teams makes the playoffs.

* **Jhansi, def not Matt:**  
If Jhansi wins she'll automatically be in the playoffs, but if she loses, she'll need some help.  A loss puts her record at 3-6, tying her with the loser of the SchlossBot vs. Tibbers matchup.  That would mean the tiebreaker goes to the team with the most total points for season.  Jhansi has 898.5 and SchlossBot has 956.5.  Therefore, she can advance if both she and SchlossBot lose, but she would need to outscore SchlossBot by more than 58 points.

* **Curmudgeonly SchlossBot:**  
The evil SchlossBot either needs to beat Alyx, or hope that Jhansi loses.  Since SchlossBot currently has 58 more total points than Jhansi, he would mostly likely win the tiebreaker if they finish with the same record.

* **Tibbers Domination:**   
Alyx will automatically advance with a win or tie against SchlossBot this weekend.  She can also advance with a loss, as long as Jhansi loses too.  Since Alyx is way ahead in total points, she would almost certainly win the tiebreaker.





###Calculating the Probability of Winning
Now that we know the scenarios for those teams to enter the playoffs, lets determine the probability of those scenarios actually happening. At first I thought it would be to good to use an ELO scoring system like those used for FIFA, college football, chess grandmasters, and League of Legends players.  However, Fantasy Football is unique in that the two opponents' scores are independent of each other (like golf).  That makes it easier to predict how many points a team will score without having to think about the quality of their opponent.  

Assuming the quality of a fantasy team stays relatively stable over time (a ridiculous assumption), then we should be able to predict a teams performance based on their past performance. We can test if thats the case comparing the average score of each team weeks 1 through 9 to their scores in week 10.

Here's a table of the scores for each week and a plot of the scores on Week 10 compared to teams' average score.

|                        | week1| week2| week3| week4| week5| week6| week7| week8| week9| week10|
|:-----------------------|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|------:|
|20% effort 80% victory  |  69.0| 116.5| 113.5| 123.0|  96.5|  90.5| 107.0| 151.0| 124.0|  111.5|
|Curmudgeonly SchlossBot |  99.5|  81.5|  81.5| 118.5|  95.0| 126.0|  83.5|  79.0|  88.0|  104.0|
|Flossin' RARAs          | 117.0| 127.0| 122.5|  93.5| 115.0| 104.0|  96.0| 122.0| 100.5|  148.0|
|Free Lunch              |  85.0|  76.0| 153.0|  89.0|  97.5| 131.0| 110.0| 138.5| 107.0|  111.5|
|Jhansi, def not Matt    |  92.0| 104.0|  98.0|  65.5| 110.5| 121.5|  80.0|  88.0|  41.0|   98.0|
|Team Didit4thelulz      | 139.0|  60.5| 137.0| 128.5| 103.0|  55.5| 126.5|  80.0| 113.5|   89.0|
|Team McCrone            |  72.0| 103.5|  73.0| 107.0|  86.5|  99.0|  67.0|  71.5|  68.0|   73.0|
|Team U-MICHed me!       |  95.0| 116.5| 104.0| 149.0| 137.5|  97.0|  66.0|  89.5| 104.5|  127.5|
|Team USA                | 111.5|  91.5| 102.5|  89.5|  81.5|  83.0| 124.5|  93.5|  89.0|  128.5|
|Tibbers Domination      | 129.5| 124.5| 103.0| 110.0| 126.5| 124.5|  80.5| 133.0| 104.0|  109.0|

![center](/../figs/2014-11-13-SFFL-Playoff-Scenarios/correlation-1.png) 

It looks like past performance is an okay predictor of a teams score.  There's a correlation coefficient of 0.57 and a p-value of 0.085.  It's not exactly a perfect correlation or statistically significant, but it's good enough for a blog post that nobody's going to read about fantasy football league that maybe 6 people care about.  By assuming teams scores are stable and normally distributed we can use the mean and variance of each teams scores to determine the probability that Team A will score more points than Team B.



{% highlight r %}
winProb <- function(team1,team2,margin=0){ #calculates the probability that team1 will beat team2 by a given margin
  mu <- mean(byTeam[team1,]) - mean(byTeam[team2,]) #mu is the mean difference in scores betwen the Teams 1 and 2
  sigma <- sqrt(sd(byTeam[team1,]) + sd(byTeam[team2,])) #sigma is their combined variance
  phi <- 1-(mu/sigma)*-1 #phi is the distribution of the differences in score
  prob <- 1-pnorm(margin, mean=mu, sd=sigma^2) #then we can calculate the probability of Team1 beating Team2
  return(prob)
}
{% endhighlight %}

My ineptitude with stats and the unrealistic assumptions I've made pretty much invalidate this mehthod, but lets roll with it.  Here's the probability of each bubble team making the playoffs...

* **Tibbers Domination:**  
The only scenario where Alyx doesn't advance is if she loses AND Jhansi wins. Therefore the probability of Alyx making the playoffs is equal to 1 minus the probability of both of those things happening.  By my calculations, the probability of Alyx losing to the SchlossBot is 0.282, and the probability of Jhansi winning is 0.347. Thereforethe probability of Alyx making the playoffs by any means is **0.902**.

* **Curmudgeonly SchlossBot:**  
We'll do the same thing for the SchlossBot except that we have to keep in mind the total Points For tiebreaker, which makes things a little trickier. For SchlossBot to make the playoffs, he would have to beat Alyx OR have Jhansi lose and be within 58 points of her score.  We can add the probabilities of those two scenarios to calculate SchlossBots chances.  The odds of beating Alyx is 0.282. The combined probability that both SchlossBot and Jhansi lose (0.469), and SchlossBot stays ahead in total points(0.947) is 0.444.  Adding those probabilities makes SchlossBot's chance of making the playoffs by any means **0.726**.

* **Jhansi, def not Matt**  
Jhansi needs to either win her game OR hope that SchlossBot loses and beat him by more than 58 points.  The probability that she wins is 0.347.  The probability of the other scenario is 0.0251.  Her total probability is equal to those two added together (**0.372**.


BAM!!! Check out those numbers.  If I did everything right, the probabilities in bold should add up to 2, since 2 out of 3 teams will make it.  Now all that's left to do is to fix your line-ups and get ready for the playoffs. And in case you missed the first one, here's the full version...

<iframe width="420" height="315" src="//www.youtube.com/embed/U7fjDS0jKiE" frameborder="0" allowfullscreen></iframe>

