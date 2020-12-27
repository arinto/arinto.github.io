---
title: "Flume Event Scalability Analysis"
date: "2012-07-09"
categories: 
  - "computing"
  - "school"
tags: 
  - "apache-flume"
  - "distributed-computing"
  - "flume"
  - "sds"
---

This is the follow up of the project in this [post](http://www.otnira.com/2012/06/16/flume-based-independent-news-aggregator/). Our professor asked us to perform scalability analysis of the technology that we used, and write simple report on it. I decided to analyze Flume scalability in term of the number of events that can be supported by the Flume configuration. The project itself is inspired from [Mike Percy's Flume-NG performance measurement](https://cwiki.apache.org/FLUME/flume-ng-performance-measurements.html), and I re-used some of his software components. There are two main differences between this project and Mike's work, which are:

-  This experiment introduces one-to-one relationship between the nodes and Flume load generator. Tht means, each Flume load generator process exists in an independent node (which is Amazon EC2 medium instance).
- This experiment introduces cascading setup, which will verify whether there is improvement in scalability or not compared to non-cascading setup

### Setup 1: One-to-one relationship between nodes and Flume load generator

Figure below shows the diagram of this setup:

\[caption id="attachment\_368" align="aligncenter" width="574"\][![Setup 1](images/Setup1Project.png "Setup 1")](http://www.otnira.com/wp-content/uploads/2012/07/Setup1Project.png) Setup1: One to one relationship between nodes and Flume  
load generator\[/caption\]

TCP Hammer sends TCP events with configurable size. In this case, we set TCP event size to 300 bytes. Flume node is configured with SyslogTcpSource which will listen to TCP event in configurable port and generate its own Flume event to be transmitted via Memory channel to the HDFS sink. The HDFS in this setup consists of three replicated nodes. We will check the relation between channel capacity and maximum rate of events that Flume node can support.

### Setup 2: Cascading Setup

Figure below shows the diagram of Cascading Setup:

\[caption id="attachment\_369" align="aligncenter" width="540"\][![Setup 2](images/Setup2Project-1024x387.png "Setup 2")](http://www.otnira.com/wp-content/uploads/2012/07/Setup2Project.png) Setup 2: Cascading setup\[/caption\]

We still used event size of 300 bytes. Two Flume scale nodes are aggregated into one single collector node before writing the data into HDFS cluster. Two separate load generators reside in different independent nodes. We will compare accumulated number of event-per-seconds of two Flume scale nodes with first setup with same settings of channel capacity.

### Experiment Result

We inspected Flume log file in /var/log/flume-ng/flume.log to check whether the configuration is able to support given event rates. If Flume process can continue without Java exceptions that cause it to stop or without regularly generated exceptions, then we will treat it as able to handle the event rates applied to it.

#### Setup 1 Result

[![Setup 1 Result](images/Table1-capture.png "Setup 1 Result")](http://www.otnira.com/wp-content/uploads/2012/07/Table1-capture.png)

Table 1 shows the result for Setup 1. In this experiment, we increased the channel capacity and we also observed that by doubling the channel capacity, the maximum events per seconds increase much less. The increment is only around 25% when we doubled from 100000 to 200000, and it's only 10% when we increased the channel capacity from 200000 to 400000.

#### Setup 2 Result

In setup 2, we used channel capacity of 100000 in Scale1-1 and Scale1-2 nodes, and we used channel capacity of 200000 in Collector node.

[![Setup 2 Result](images/Table2-capture.png "Setup 2 Result")](http://www.otnira.com/wp-content/uploads/2012/07/Table2-capture.png)

From Setup 1 experiment, we found that maximum events that can be supported is 200. In setup 2, we can easily see that the cumulative maximum event rates are 400. And it is very much expected because we increased the resource by adding new nodes with more processing resources. Note that when event arrives in node Scale1-1 or Scale1-2, the event is not directly forwarded into Collector node. Node Scale1-1 and Scale1-2 will hold the events until certain number of events are accumulated. This amount of accumulated events are configurable using flume.conf configuration file.

Well, actually there are some perl scripts using Pig in Mike Percy's codes that can be used to process the data further. The scripts are utilized to analyze the data in HDFS and provide more insightful result such as total number of retrieved events and total number of retry in sending events. Unfortunately, due to limited time of experiment, we did not able to make scripts work. We have tried to hack the scripts and HDFS configuration, but we still receive error message when the scripts are executed against the HDFS data.

In conclusion, Flume is promising in term of scalability. By using cascading setup, we are able to improve the scalability of Flume. However, more analysis is needed especially using additional existing Pig script to process the data. :D.

Well that's it for this project. Really wish to have more time to hack the Pig script and make the additional data analysis works! :) For actual report, please refer to the link below: http://www.slideshare.net/arinto/flume-event-scalability
