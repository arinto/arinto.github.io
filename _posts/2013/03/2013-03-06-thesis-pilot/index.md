---
title: "Thesis: Pilot"
date: "2013-03-06"
categories: 
  - "projects"
  - "thesis"
tags: 
  - "distributed-streaming-machine-learning-framework"
  - "machine-learning"
  - "project"
  - "streaming"
  - "thesis"
---

After one and half month starting my master thesis, finally I have chance to start writing about it. And after getting the permission from one of my supervisors, [Gianmarco](http://gdfm.me/ "Gianmarco's blog"), I can publish this post, yay!

In this pilot post, I would like to give overview of the thesis. In a nutshell, the thesis is about achieving **high velocity** in big data analytics, by developing **distributed streaming machine learning framework**. So, without further ado, here is the overview. :D

\[caption id="attachment\_704" align="aligncenter" width="271"\][![Yes, my thesis is related to big data analysis](images/big-data-cartoon-271x300.jpg)](https://secure.flickr.com/photos/t_gregorius/5839399412/in/photostream) Yes, my thesis is related to big data analysis\[/caption\]

\*the above cartoon image is taken from [Space & Light's Flickr](https://secure.flickr.com/photos/t_gregorius/)

### **The Overview**

Everyone is talking about [big data](http://en.wikipedia.org/wiki/Big_data "Big Data") and one of the initial questions surrounding big data is _"how to store it?"_.  Single piece of disk is not able to store the big data. Customized solutions with specialized and expensive hardware were the only choice to handle it until Google published [GFS](http://research.google.com/archive/gfs.html "GFS"), which allowed Google to build infrastructure to store big data using off-the-shelf component. Open Source community was inspired by GFS, and  it responded with the creation of [Hadoop's](http://hadoop.apache.org/ "Hadoop") HDFS. Until now, HDFS is widely used by many [corporations](http://wiki.apache.org/hadoop/PoweredBy "Hadoop powered by"). Other than GFS and HDFS, there are other distributed storage system such as Google's [BigTable](http://research.google.com/archive/bigtable.html "Big Table"), Facebook's [Cassandra](http://en.wikipedia.org/wiki/Apache_Cassandra) , Amazon's [Dynamo](http://dl.acm.org/citation.cfm?id=1294281 "dynamo") and many more.

The next natural question after successfully storing big data is _"how to process it?"_.  From system-point-of-view, we have Google's [MapReduce](http://research.google.com/archive/mapreduce.html "Map Reduce") which inspired open-source version also called [MapReduce](http://wiki.apache.org/hadoop/MapReduce "Apache Map Reduce"). For graph processing we have Google's [Pregel](http://dl.acm.org/citation.cfm?id=1807184 "Pregel"), which inspired Apache [Giraph](http://incubator.apache.org/giraph/). From analytics-point-of-view, we have statistics, machine learning (ML) and data mining. They are close to each other, but they are not the same. Statistics is about the mathematical techniques behind data analytics. With regards to big data, the concept should not change but the implementations may change i.e. the implementation should be able to scale to handle the magnitude of big data. And,so do ML and data mining, the concept behind them do not change, but the implementations need to be revisited in order to process big data properly. To address this challenge, Open Source community created [Mahout](http://mahout.apache.org/ "Mahout"), distributed ML framework on top of Hadoop. Other than Mahout, there are also [Distributed Graphlab](http://dl.acm.org/citation.cfm?id=2212354 "Graphlab") for graph-based ML, and [MLbase](https://amplab.cs.berkeley.edu/projects/mlbase/ "ML Base") which part of [Berkeley's Data Analytics Stack](http://ampcamp.berkeley.edu/wp-content/uploads/2013/02/Berkeley-Data-Analytics-Stack-BDAS-Overview-Ion-Stoica-Strata-2013.pdf).

So far we are able to store big data and process the stored data. Well, are we satisfied now? Are these enough for big data? Of course the answer is No! Now, we want to be able to improve analytics speed by performing analysis on continuous supply of data. And this is where [streaming paradigm](http://en.wikipedia.org/wiki/Data_stream_mining "Data stream mining") comes into the picture. We want to perform streaming data analytics using ML. Up until now, we already have streaming ML framework such as [MOA](http://moa.cms.waikato.ac.nz/ "MOA") and [Vowpal Wabbit](http://hunch.net/~vw/ "Vowpal Wabbit"). They utilize streaming ML techniques which optimized handling data streaming, but they are not designed for scalability to handle big data. These conventional frameworks have risk of performance degradation when processing very high streaming rate or processing data with huge number of ML attributes. Other than the streaming ML frameworks, we also have distributed stream processing platform such as [S4](http://incubator.apache.org/s4/ "S4") and [Storm](http://storm-project.net/ "Twitter Storm"). However, there is only a few ML implementations on top of them. MOA could be integrated with S4 and Storm as MOA's data stream source, but MOA itself only runs on a single machine, hence it does not scale and it could be the performance bottleneck when performing big data analytics.

Therefore, the proposed thesis here is to solve the scalability issue and the risk of performance degradation in  **streaming ML frameworks** by tapping the power of existing **distributed stream processing** platforms. This thesis will in allow high velocity in big data processing. We are planning to materialize our aforementioned thesis into a **distributed streaming ML framework**.
