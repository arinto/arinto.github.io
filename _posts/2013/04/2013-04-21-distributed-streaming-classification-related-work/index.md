---
title: "Distributed Streaming Classification: Related Work"
date: "2013-04-21"
categories: 
  - "computing"
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "decision-tree"
  - "distributed-computing"
  - "distributed-machine-learning"
  - "distributed-streaming-machine-learning"
  - "machine-learning"
  - "parallel-machine-learning"
  - "streaming-machine-learning"
  - "thesis"
---

In this post, I plan to write some quick recap of related works in Distributed Streaming Classification, focusing on decision tree induction. It is still related to my [thesis](http://www.otnira.com/2013/03/06/thesis-pilot/ "Thesis: Pilot") in Distributed Streaming Machine Learning Framework. I divide this post into four sections: Classification, Distributed Classification, Streaming Classification, and Distributed Streaming Classification. Without further ado, let's start with Classification

### **Classification**

Classification is a type machine learning task which infers a **function** from **labeled training data**. This function is used to **predict the label (or class)** of testing data. Classification is also called as _supervised learning_ since we use the actual class output (the ground truth) to supervise the output of our classification algorithm. Many classification algorithms have been developed such as tree-based algorithms (C4.5 decision tree, bagging and boosting decision tree, decision stump, boosted stump, random forest etc), neural-network, Support Vector Machine (SVMs), rule-based algorithms(conjunctive rule, RIPPER, PART, PRISM etc), naive bayes, logistic regression and many more.

These classification algorithms have their own advantages and disadvantages, depending on many factors such as the characteristics of analyzed data and results. For more details about classification algorithm characteristics, you can start by checking [Caruana and Niculescu-Mizil's work](dl.acm.org/ft_gateway.cfm?id=1143865&type=pdf "Empirical Comparison of Supervised Learning") where ten supervised learning methods were empirically evaluated and compared. They followed up the work with [further evaluation and comparison in high-dimensions data](dl.acm.org/ft_gateway.cfm?id=1390169&type=pdf "Comparison in High Dimension Data"). And with regards to decision tree induction, C4.5 decision tree induction belongs to this category.

### **Distributed and parallel Classification**

Classifications (and also machine learning in general) always assume that the training and testing data are available in the **memory**, hence the associated algorithms can easily perform data analysis on it (such as perform **multiple passes** on training data to enhance machine learning model). However, the abundance of data and the need to process larger amounts of data have triggered machine learning development. Classic classification algorithms are modified into **scaled-up versions**, i.e. executing the algorithm in multiple machines with message-passing or shared-memory paradigm or expanding the limited memory with temporary external storage.

Distributed and parallel machine learning frameworks are pretty common nowadays such as: [GraphLab](http://arxiv.org/pdf/1006.4990.pdf "GraphLab") & [Distributed GraphLab](dl.acm.org/ft_gateway.cfm?id=2212354&type=pdf "Distributed GraphLab"), [Pregel](dl.acm.org/ft_gateway.cfm?id=1807184&type=pdf "Pregel") and [MLBase](https://sites.google.com/site/mlbaseproject/home "MLBase"). There are also some works that try to utilize MapReduce as for distributed machine learning such as [Gillick et. al. paper](citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.111.9204&rep=rep1&type=pdf "Gillick"). With regards to decision tree induction, these are the publications that propose distributed decision tree induction:

1. PLANET: massively parallel learning of tree ensembles with MapReduce (Panda et. al., [paper](dl.acm.org/ft_gateway.cfm?id=1687569&type=pdf "PLANET")). It used task parallelism of decision tree induction using master (controller) and worker model.
2. Stochastic gradient boosted distributed decision trees (Ye et. al., [paper](dl.acm.org/ft_gateway.cfm?id=1646301&type=pdf "Stochastic GBDT")). The authors implemented two version of distributed decision trees: using horizontal-data-partitioning on top of MapReduce and using vertical-data-partitioning on top of MPI on Hadoop.
3. Characterization and parallelization of decision-tree Induction (Bradford and Fortes, [paper](https://www.sciencedirect.com/science/article/pii/S0743731500916927 "Bradford & Fortes"))
4. Chi-square test based decision trees induction in distributed environment(Ouyang et. al, [paper](ieeexplore.ieee.org/ielx5/4733896/4733897/04733971.pdf?tp=&arnumber=4733971&isnumber=4733897 "Chi-square").)

### **Streaming Classification**

Another development of machine learning is in processing **continuous supply of data**. In conventional classification and machine learning implementations, an induced model (which is the training result), is not designed to be updated with the arrival of new data. The conventional training needs to be performed again from the beginning with the new arrived data and it is costly and time-consuming. This characteristic is often undesirable because it causes the overall machine learning and classification algorithm slows and could not handle the continuous supply of data.

However, fret not! _Data stream paradigm_ has emerged to address the aforementioned continuous data supply challenge. And as I explained in my [previous post](http://www.otnira.com/2013/03/28/hoeffding-tree-for-streaming-classification/ "Hoeffding Tree for Streaming Classification"), there are two main characteristics of data stream paradigm: high data volume & rate, and unbounded. And there are four characteristics of streaming-machine-learning-implementation: process example at a time & inspect it exactly once, use limited amount of memory, bounded processing time, able to predict at any time.

Here are some of the **machine learning frameworks** that aim to handle data stream paradigm are

1. [MOA](http://moa.cms.waikato.ac.nz/ "Massive Online Analysis"): Massive Online Analysis. It's a Java-based framework containing classification, clustering, and regression streaming algorithm implementation. ([overview](http://moa.cs.waikato.ac.nz/overview/ "MOA overview"), [manual](http://heanet.dl.sourceforge.net/project/moa-datastream/documentation/Manual.pdf "MOA manual"))
2. [Vowpal Wabbit](http://hunch.net/~vw/ "Vowpal Wabbit"). It is a C++ based framework which uses online gradient descent as its principal learning algorithm.
3. [Debellor](http://www.debellor.org/ "Debellor"). Another Java-based machine learning framework that aims for scalable data mining through data streaming architecture. ([paper](http://www.debellor.org/debellor.pdf "Debellor"))

With regards to decision tree induction, here are some publications that propose or discuss streaming decision tree and/or machine learning

1. Mining high-speed data streams (Domingos and Hulten, [paper](http://doi.acm.org/10.1145/347090.347107 "Domingos and Hulten")). The authors proposed streaming decision tree induction based on **Hoeffding bound**, and implemented their proposed as VFDT (Very Fast Decision Tree) learner.
2. Efficient decision tree construction on streaming data (Jin and Agrawal, [paper](http://doi.acm.org/10.1145/956750.956821)). The paper is about another proposal on streaming decision tree induction.
3. Mining time-changing data streams (Hulten et. al., [paper](http://doi.acm.org/10.1145/502512.502529)). The authors proposed Streaming decision tree learner that reacts properly with **time-changing data streams** i.e. data stream that can change its characteristics so that the current learner is not suitable.
4. A streaming ensemble algorithm (SEA) for large-scale classification (Street and Kim, [paper](http://doi.acm.org/10.1145/502512.502568)).
5. Mining data streams: a review (Gaber et. al., [paper](http://doi.acm.org/10.1145/1083784.1083789))
6. Issues in evaluation of stream learning algorithms (Gama et. al., [paper](http://doi.acm.org/10.1145/1557019.1557060))

### **Distributed Streaming Classification**

Nowadays the amount of data consumed by web-companies has reached petabytes scale. This type of data is commonly called Big Data. Other than magnitude, the data velocity of the data is also increasing significantly. Existing machine learning frameworks in previous section are designed to be executed in single-machine settings hence they are likely not able to reach high velocity big data processing.

To achieve this high-velocity requirement, we propose to build **distributed streaming machine learning framework** (which is my thesis :D).

There is not many related works in distributed or parallel streaming machine learning framework but so far I managed to obtain the following references:

1. [Jubatus](http://jubat.us/en/ "Jubatus"). Scalable distributed online machine learning framework for **real time big data analysis**. Developed by Makino from NTT Software Innovation center. (check out the [slides](http://www-conf.slac.stanford.edu/xldb2012/talks/xldb2012_wed_LT09_HMakino.pdf "jubatus slides") from XLDB2012 to get high-level-view of Jubatus)
2. A streaming parallel decision tree algorithm (Ben-Haim and Tom-Tov, [paper](http://jmlr.csail.mit.edu/papers/volume11/ben-haim10a/ben-haim10a.pdf "ben-haim_tom-tov")). The authors proposed approximate algorithm for streaming data to build decision tree. The advantages of their algorithm are its ability to be executed in parallel, hence it is faster compared to serial implementation without sacrificing the error-rate performance

### Wrap-up!

Well, that's all for the related works for now. I may add more during my actual thesis writing. Hohoho. For next post, I'm planning to discuss about Twitter Storm. So, stay tune!

![](images/dtipIconHover.png)
