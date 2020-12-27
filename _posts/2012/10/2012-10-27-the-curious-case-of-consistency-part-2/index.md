---
title: "The Curious Case of Consistency - Part 2"
date: "2012-10-27"
categories: 
  - "computing"
  - "talk"
tags: 
  - "bounded-staleness"
  - "cap"
  - "consistency"
  - "consistency-model"
  - "consistent-prefix"
  - "distributed-computing"
  - "eventual-consistency"
  - "monotonic-read"
  - "read-your-write"
  - "strong-consistency"
---

Arggghh.. I broke my promise!! I should have finished this post earlier.. :(. huffff..  I was busy with school assignments and activities with Indonesian societies in Stockholm hehe.. maybe I should write on it as well humm... okay, now back to business :)

In the [previous post](http://www.otnira.com/2012/09/18/the-curious-case-of-consistency/ "The Curious Case of Consistency – Part 1"), I wrote about several consistency types from [Doug Terry](http://research.microsoft.com/en-us/people/terry/ "Doug Terry")'s breakfast talk in my school. Now, it's time to see their application in simple baseball game.

## Simple Baseball Game

The baseball game itself will consist of several "entities" that are "interested" in the latest score of the game. The "entities" are represented as pseudocode, and the term "interested" can be interpreted as read or write depending on entity type. We will discuss what kind of consistency that is needed for each entity below

### 1\. The game

\[caption id="attachment\_481" align="aligncenter" width="349"\][![](images/TheGame.png "Baseball Game")](http://www.otnira.com/wp-content/uploads/2012/10/TheGame.png) Pseudocode of Baseball Game\[/caption\]

The main-point-to-note in this pseudocode is the way the scores are updated. They are read first before updated. Although each read and write is atomic, the whole updating process is not atomic (and may exhibit some problems when multiple different threads start accessing it)

### 2\. Official Scorekeeper

\[caption id="attachment\_477" align="aligncenter" width="254"\][![Score-keeper](images/Scorekeeper.png "Scorekeeper")](http://www.otnira.com/wp-content/uploads/2012/10/Scorekeeper.png) Pseudocode for Score Keeper\[/caption\]

Since there is only one scorekeeper, read-your-write consistency model is enough to ensure that the scores are updated correctly.

### 3\. Umpire

\[caption id="attachment\_482" align="aligncenter" width="262"\][![Umpire](images/Umpire.png "Umpire")](http://www.otnira.com/wp-content/uploads/2012/10/Umpire.png) Pseudocode for umpire\[/caption\]

Consistent-prefix model is enough in this case. We just need to ensure that umpire has seen the data up to 9th inning to perform. But this model may provide some strange case where the game theoretically is over, but still on going because umpire has not seen the latest data.

However, Doug argues that strong-consistency is needed to ensure umpire receives up-to-date information.

### 4\. Radio Reporter

\[caption id="attachment\_476" align="aligncenter" width="294"\][![Radio Reporter](images/RadioReporter.png "RadioReporter")](http://www.otnira.com/wp-content/uploads/2012/10/RadioReporter.png) Pseudocode for Radio Reporter\[/caption\]

Reporter is only perform reading and his role is not really critical. However, we need to ensure that he doesn't report data that is older than what he previously reports (example: at t1, he reports hScore = 3, then at t2, and t2 > t1, he reports hScore = 2), so monotonic-read model is appropriate for this case.

Consistent-prefix is not enough for this case because consistent-prefix only guarantees that the reader will read valid data some-time in the past, which may imply he read vScore & hScore as 2:5 at time t1, then he read vScore & hScore as 1:3 at time t2, where t2 > t1. Note that in this case both combinations of score are valid in the past.

Another option is bounded-staleness model, where the staleness time is set to 30 minutes. This implies reporter will be receive data which up to date up to 30 minutes in the past.

### 5\. Sportswriter

\[caption id="attachment\_478" align="aligncenter" width="263"\][![Sports Writer](images/SportsWriter.png "SportsWriter")](http://www.otnira.com/wp-content/uploads/2012/10/SportsWriter.png) Pseudocode for Sportswriter\[/caption\]

Sportswriter reads the data sometimes in the future after the game end (well, it maybe 1 or 2 hours after the game finished), and he only wants the latest scores. In my opinion, "reasonable" eventual consistency is enough. What I  mean by "reasonable" here is related to the actual time to propagate updates to all replicas, which must be kept under 1 hour.

Doug makes very interesting analysis in his [paper\[1\]](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf "Consistency and Baseball Report"). He mentions that sportswriter wants the result of strong-consistency (which is the most updated score), but actually he does not need to pay the cost (since he does not need the result immediately). Therefore he proposes eventual consistency or bounded-staleness consistency with 1 hour threshold.

### 6\. Statistician

\[caption id="attachment\_479" align="aligncenter" width="306"\][![Statistician](images/Statistician.png "Statistician")](http://www.otnira.com/wp-content/uploads/2012/10/Statistician.png) Pseudocode for Statistician\[/caption\]

Statistician needs to get the latest and most up-to-date score right after the game. Therefore, if the time between end of the game and reading the scores are limited, strong consistency is needed here. However, if there is some time gap between end of the game and reading the scores (as in sportswriter case), then bounded-staleness consistency model is enough.

Assuming there is only one statistician for each team that read and write seasons-runs data, reading the seasons-runs data can be performed by using read-your-write model as in official scorekeeper consistency model.

### 7\. Stat Watcher

\[caption id="attachment\_480" align="aligncenter" width="307"\][![Stat Watcher](images/StatWatcher.png "Stat Watcher")](http://www.otnira.com/wp-content/uploads/2012/10/StatWatcher.png) Pseudocode for Stat Watcher\[/caption\]

Stat watcher access the seasons-runs data one time everyday (every 24 hours), this means there will be ample amount of time for seasons-runs update to be propagated to  all replicas (24 hours are considered LOTS of time to propagate updates . Hence, eventual-consistency is enough in this case.

## Conclusion

As we can see here, all consistency models are useful, and the choices of consistency really depend on the characteristics of the clients/applications. Therefore, we need to choose wisely appropriate consistency model based on how applications use the data, although they use the same data set. :)

Well, that's all I have for this post. Comments and suggestions are welcomed :)

#### References:

_\[1\] D. Terry, “Replicated Data Consistency Explained Through Baseball,” Oct-2011. \[Online\]. Available:[http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf). \[Accessed: 18-Sep-2012\]._
