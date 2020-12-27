---
title: "Hoeffding Tree for Streaming Classification"
date: "2013-03-28"
categories: 
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "decision-tree"
  - "hoeffding"
  - "hoeffding-tree"
  - "machine-learning"
  - "streaming-machine-learning"
  - "thesis"
---

In the [previous post](http://www.otnira.com/2013/03/25/c4-5/ "C4.5 Decision Tree Implementation"), we have summarized C4.5 decision tree induction. Well, since my thesis is about [distributed streaming machine learning](http://www.otnira.com/2013/03/06/thesis-pilot/ "Thesis: Pilot"), it's time to talk about **streaming** decision tree induction and I think it's better start with defining **"streaming** **machine learning**" in general.

### **Streaming Machine Learning**

Streaming machine learning can be interpreted as performing machine learning in **streaming** setting. In this case, streaming setting is characterized by:

- High data volume and rate, such as transactions logs in ATM and credit card operations, call log in telecommunication company, and social media data i.e. Twitter tweet stream or Facebook status update stream
- Unbounded, which means these data always arrive to our system and we won’t be able to fit them in memory or disk for further analysis with the techniques. Therefore, this characteristic implies we are limited to analyse the data once and there is little chance to revisit the data

Given these characteristics, conventional machine learning algorithms (which requires all the data to be available in memory) are not suitable to handle it. The requirements to handle streaming setting vary between methods. For classification algorithms, they need to adhere these following four requirements:

1. Process an example at a time and inspect it only once (at most)
2. Use limited a mount of memory
3. Work in limited amount of time
4. Be ready to predict at any point

Another aspect of streaming machine learning is change detection i.e. since our input data is unbounded, we need to have mechanism to handle and react to changes in incoming data characteristics. In classification, this aspect yields questions such as

- should we modify our classifier to fit data characteristics changes?
- what kind of modification that should we perform?

For more detailed explanation about change detection on streaming classification, you can read the first chapter of [Data Stream Mining: A Practical Approach e-book](http://sourceforge.net/projects/moa-datastream/files/documentation/StreamMining.pdf/download?use_mirror=garr "Data Stream Mining").

### **Hoeffding Tree**

The streaming decision tree induction is called Hoeffding Tree. The name is derived from the Hoeffding bound that is used in the tree induction. The main idea is, Hoeffding bound gives certain level of confidence on the best attribute to split the tree, hence we can build the model based on certain number of instances that we have seen.

Without further ado, here is the summary from Domingos and Hulten's paper;["Mining High Speed Data Stream"](https://dl.acm.org/citation.cfm?id=347107 "domingos-hulten-mining-stream"); where they propose Hoeffding Tree.

#### Motivation

Existing streaming classification techniques have some shortcomings such as:

1. Highly sensitive to example ordering
2. Low efficiency. In some cases, they are slower than batch algorithm

Solving the challenge of high speed continuous data stream processing and the shortcomings of existing techniques will produce more insight and better analysis into continuous data stream. “Better analysis” in this context refers to quality as well as the efficiency of the analysis (i.e. fast analysis which is able to cope with incoming data rate)

#### Contributions

The main contribution is Hoeffding-tree, which is a new decision-tree learning method for streaming that solves these following challenges:

1. Uncertainty in learning time. Learning in Hoeffding tree is constant time per example (instance) and this means Hoeffding tree is suitable for mining data streaming.
2. The resulting trees are nearly identical with trees built by conventional batch learner, given enough example to train the and build the Hoeffding trees

#### Hoeffding Bound

To achieve the streaming classification characteristics (the 4 characteristics that are mentioned in Streaming Machine Learning section abaove), the authors introduce Hoeffding bound to decide how many example of instances needed to achieve certain level of confidence (i.e. the chosen instance attribute using the bound is the close to the attribute chosen when infinite examples are presented into the classifier).

Given:

- $latex r $ = real-valued random variable, with range R
- $latex n $ = number of independent observations have been made
- $latex \\bar{r}$ = mean value computed from $latex n$ independent observations

Hoeffding bound states that with probability $latex 1-\\delta$, the true mean of the variable is at least $latex \\bar{r}-\\epsilon$. And $latex \\epsilon$ is given by the following formula:

$latex \\epsilon = \\sqrt{\\frac{R^{2}ln(1/\\delta)}{2n}}$

What makes Hoeffding bound attractive is its ability to give the same results regardless the probability distribution generating the observations. However, the number of observations needed to reach certain values of $latex \\delta$ and $latex \\epsilon$ are different across probability distributions.

Here is a concrete example to help us understanding the concept:

Let's say the information gain difference between two attributes (attribute A and B, A’s info gain is greater than B) is 0.3 and $latex \\epsilon=0.1$. This means, in the future, the minimum difference between A’s info gain and B will be at least $latex 0.3-0.1=0.2$.

In other words: With probability $latex 1-\\delta$ , one attribute is superior compared to others when observed difference of information gain is greater than $latex \\epsilon$.

#### Basic Algorithm

Figure below shows the pseudocode of Hoeffding Tree induction:

\[caption id="attachment\_815" align="aligncenter" width="518"\][![Pseudocode of Hoeffding Tree](images/HoeffdingTreeBasicAlgo.png)](http://www.otnira.com/wp-content/uploads/2013/03/HoeffdingTreeBasicAlgo.png) Pseudocode of Hoeffding Tree\[/caption\]

Here are some particular aspects of the algorithm that important:

1. Sufficient statistics. The statistics in each leaf should be sufficient to calculate information gain. It is relatively simple to store and calculate for nominal attributes, but further discussion is needed for continuous numeric attributes.
2. Grace period. Hoeffding bound is computed every $latex n\_{min}$ instances arrival because single example will have little influence to the calculation results.
3. Pre pruning. $latex X\_{\\o}$ represents the null attribute which indicates the merit of no-split. It is used for pruning the tree.
4. Tie breaking. $latex \\tau$ is used to choose in between two attributes that has very close information gain i.e, the difference is so small which implies $latex \\epsilon < \\tau $
5. Skewed split prevention. Split is only allowed, when the ratio between two branches is at least 1:99, i.e. 1% of the instances go to first branch, and the remaining 99% go to the second branch. Split will not be performed if less than 1% goes to a specific branch

#### VFDT Implementation

The authors implemented the Hoeffding tree algorithm into Very Fast Decision Tree learner (VFDT) which includes some enhancements for practical use, such as node-limiting strategy, introduction of as tie breaking parameter, grace period of bound calculation, poor attributes removal, fast initialization by using conventional RAM-based learner and ability to rescan previously-seen examples when data rate is slow.

### **Wrap up!**

The next step of my  thesis is to determine how we can make this algorithm execute in Distributed and Parallel manner. But before we started cracking the algorithm, we did sufficient review on existing distributed and parallel machine learning algorithms and frameworks. Which I plan to write on the next post. Stay tuned!

 

![](images/dtipIconHover.png)
