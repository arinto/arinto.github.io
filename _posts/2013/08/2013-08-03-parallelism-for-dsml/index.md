---
title: "Parallelism for Distributed Streaming ML"
date: "2013-08-03"
categories: 
  - "computing"
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "distribute-online-machine-learning"
  - "distributed-streaming-machine-learning"
  - "machine-learning"
  - "paralellism"
  - "thesis"
---

In this post, we will revisit several parallelism types that can be applied to modify conventional **streaming (or online) machine learning algorithms** into **distributed** and **parallel** ones. This post is a quick summary of half of chapter 4 of my [thesis](http://www.slideshare.net/arinto/emdc-thesis "EMDC Thesis") (which I completed one month ago! yay!).

## Data Parallelism

Data Parallelism parallelize and distribute the algorithms based on the data. There are two types of data parallelism, they are Vertical Parallelism and Horizontal Parallelism.

### Horizontal Parallelism

Horizontal parallelism splits the data based on the quantity of the data i.e. same amount of data subset goes into the parallel computation. If let's say we have 4 components that perform parallel computation, and we have 100 data, then each component computes 25 data. As shown in figure below, each parallel component has local machine learning (ML) model. Every parallel component then performs periodical update into the global ML model. [![Horizontal Parallelism](images/HorizontalPar.png "Horizontal Parallelism")](http://www.otnira.com/wp-content/uploads/2013/08/HorizontalPar.png)

This type of parallelism is often used to provide horizontal scalability. In online learning context, horizontal parallelism is suitable when the data arrival rate is very high. However, horizontal parallelism needs high number of memory since it needs to replicate the online machine learning model in every parallel computation element. Another caveat for horizontal parallelism is the additional complexity that introduced when propagating the model updates between parallel computation element. Example of horizontal parallelism in distributed streaming machine learning algorithm is [Ben-Haim and Yom-Tov's work](http://jmlr.csail.mit.edu/papers/volume11/ben-haim10a/ben-haim10a.pdf) about streaming parallel decision tree algorithm.

### Vertical Parallelism

Vertical parallelism splits the data based on one or more specific internal characteristics of the data. In our context, which is machine learning, we can use the attributes of the data as the characteristic to split the data. As shown in figure below, ML model splits each arriving datum based on its attribute, and distribute the split datum into the available local statistic (in the example, we have two local statistics). Each local statistic perform parallel computation based on the assigned attributes and perform periodic model updates in ML model component.

[![Vertical Parallelism](images/VerticalPar.png)](http://www.otnira.com/wp-content/uploads/2013/08/VerticalPar.png)

This type of parallelism is suitable for a machine learning algorithms that need to process data with high number of attribute, such as _documents_. A document is less rigid form of a row or a record of relational database system. It consists of a table-like data format but it is not required to comply with database schema such as foreign key and primary key. And similar to text mining, the collection of documents is implemented as a dictionary which have 10000 to 50000 entries since each significant word corresponds to one entry in the dictionary.

Vertical parallelism is not suitable for cases where the number of attributes is too small hence the communication cost of distributing the data exceeds the benefit obtained by performing the parallel computation.

We could not find examples of vertical parallelism for distributed streaming ML algorithms. Existing literature that utilizes vertical parallelism is [stochastic gradient boosted distributed decision tree by Ye et. al](http://doi.acm.org/10.1145/1645953.1646301 "SGD boosted decision tree"). This work is an example of distributed ML algorithm but not for streaming.

## Task Parallelism

Task parallelism divides the algorithms into several sub-tasks that can be executed in parallel. In term of decision tree induction, the parallel tasks are the attempt to grow the tree in each leaf.  Task parallelism explanation is pretty straightforward but the implementation varied between algorithm. As shown in figure below, the machine learning model is a decision tree, hence each circle represent a node in the decision tree. Each square represents a computation element which will be translated into a specific stream processing engine (SPE) component (note that the distributed streaming ML algorithm normally will be implemented on top of SPEs such as Storm or S4). To grow the tree,  each leaf periodically calculates the statistic in parallel (the figure below shows the leaves in ML sub-model#2 and ML sub-model#3).

[![Task Parallelism](images/TaskPar.png)](http://www.otnira.com/wp-content/uploads/2013/08/TaskPar.png)

## Hybrid Parallelism

Hybrid parallelism combines more than one type parallelisms in the distributed algorithms. For example, an algorithm may employ vertical parallelism in the beginning of the algorithm and then it changes the parallelism type to task parallelism in the latter stage of the algorithm.

##  Wrap up!

In the next post, I plan to give basic overview of the distributed streaming machine learning framework called [Scalable Advanced Massive Online Analysis (SAMOA)](http://samoa-project.net/ "SAMOA project"). This framework is part of my thesis and it will be released by Yahoo! Labs in September 2013. Stay tuned! I'll do my best to update this blog more often!
