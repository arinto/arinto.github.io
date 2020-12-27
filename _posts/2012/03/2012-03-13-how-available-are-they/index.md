---
title: "How available are they?"
date: "2012-03-13"
categories: 
  - "computing"
  - "school"
tags: 
  - "availability-prediction"
  - "distributed-computing"
  - "volunteer-computing"
---

Well, it's not really about relationship status :p.. since "they" are referring to distributed systems :D #geek

But it is about my latest review for a paper titled titled "[Exploiting Availability Prediction in Distributed Systems](http://static.usenix.org/events/nsdi06/tech/full_papers/mickens/mickens.pdf)", by James W. Mickens and Brian D Noble. As we all know, availability is one of the important properties of distributed systems. Availability is concerned with the capability of a distributed system to serving its client properly although there are some component failures inside the system.They argued that availability modeling is crucial to (generally) make the system better, in term of resource efficiency and understanding system-wide phenomenon. Therefore, they propose a new way to predict availability, and they applied the predictor to three case studies. They found that their predictor works well under test data and they successfully shows that good predictor can improve the systems.

And, I present you with the extract of my analysis of the paper according to the metrics that are given by our Professor:

_WHAT, in your opinion, are the most original contributions of this paper?_

Most original contribution is combination of several availability predictor techniques, called Hybrid predictor with tournament counters

_WHAT, in your opinion, is the significance of the paper?  What aspects of the paper are likely to be used by other researchers and/or practitioners?_

The main significance of this paper is the implementation of Hybrid predictor and the measurement results using Microsoft nodes, [Planet Lab](http://www.planet-lab.org/) and [Overnet](http://en.wikipedia.org/wiki/Overnet).

_What was the most challenging, but important, thing to understand about the paper? Give an example of the concept, what your confusions were/are, and why it is important. (In other words, don’t choose something that was challenging because it was poorly explained, but is also not important. Choose something that IS important but that was a difficult concept to grasp)_

The explanation about hybrid predictor’s tournament counter is important, because it is the main idea of this paper and it is something new that this paper proposes.

Tournament counter is the method to determine the availability of nodes by combining several availability techniques presented before. The availability predictions techniques used here are

1. Saturating Counter Predictor (RightNow)
2. Generalized Saturating Counter Predictor (SatCount)
3. State-based Predictor (History)
4. State-based Predictor with capability of tweaking predictions using states-superpositioning (TwiddledHistory)
5. Linear Predictor

The Hybrid Predictor can be depicted as below

\[caption id="" align="aligncenter" width="418" caption="Hybrid Prediction employs the tournament-like mechanism to decide which predictor is chosen for current period"\]![Hybrid Prediction](images/HybridPrediction.PNG "Hybrid Prediction scheme from the paper")\[/caption\]

Based on the enqueued predictions, hybrid predictor will compare with current online status and update the tournament states. In the example above, the positive value of each stage of tournament counter will choose the predictor on the right. However, negative value will choose the predictor on the left side. The tournament counter in each stage is incremented and decremented appropriately based on the correctness of predictor output with the current online status of the system.

_Please detail any technical errors, uncited related work, and/or ways in which the technical material could be improved._

Add discussion/analysis about overhead created by availability predictor in the overall system. Possible question to be discussed here is: In which circumstances that the overhead from predictor outweigh its benefit?

_Please summarize the paper in your own words (~100 words) and very few sentences_

The paper presents a novel way to predict availability in loosely-coupled distributed system. It is called Hybrid Predictor and combines several sub-predictors such as:

1. Saturating Counter Predictor (RightNow). Requires only one bit of state, node’s current online status is used as the value of all predictions for all lookahead periods.
2. Generalized Saturating Counter Predictor (SatCount). It is generalized version of RightNow. That means we’re extending the state not only for one bit, but also for n bits of state.
3. State-based Predictor (History). It represents node state by using De Bruijn graph. And use the graph traversal to predict the availability.
4. State-based Predictor with capability of tweaking predictions using states-superpositioning (TwiddledHistory). It is history prediction with additional superpositioning feature. Superpositioning here means combination of multiple uptime states in such a way so that we can tweak the prediction toward probabilistically favoured ones.
5. Linear Predictor. It is the application of linear combination of the last k signals to predict future point. This technique is claimed to be good estimates for signals (in this case it is the availability) that stable in short term but oscillatory in the medium or long term.

Hybrid predictor combines the five sub predictors above using tournament counter as explained in question and figure above.

The paper also presents three examples of availability predictor application. Based on the experiment results that they have, their availability predictor improves the performance of the presented examples.

Well that's it from me :D. Comments and suggestions are welcomed!
