---
title: "Apache Flume"
date: "2012-03-20"
categories: 
  - "computing"
  - "school"
tags: 
  - "data-collection-service"
  - "distributed-computing"
  - "eedc"
  - "flume"
---

This time, our group needed to prepare presentation about [Apache Flume](https://cwiki.apache.org/FLUME/) for EEDC homework. Flume is intended to solve challenges in safely transferring huge set of data from a node (example: log files in company web servers) to data store (example: HDFS, Hives, HBase, Cassandra etc etc).

\[caption id="" align="aligncenter" width="170" caption="Apache Flume"\]![Apache Flume](images/flume-logo.jpeg "Apache Flume")\[/caption\]

Well, for a simple system with relatively small data set, we usually customize our own solution to do this job, such as to create some script to transfer the log to database. However, this kind of ad-hoc solution is difficult to make it scalable because usually it is created very tailored into our system. It sometimes suffers from problem in manageability, especially when the original programmer or engineer who created the system left the company. It is also often difficult to extend and, furthermore it may have problem in reliability due to some bugs during the implementation.

And Apache Flume comes into the rescue!!!

Apache Flume is a **distributed data collection service** that gets flows of data (like logs) from their source and aggregates them as they are processed. It is based on the concept of data flow through some Flume nodes. In a node, data come from a _source_, optionally processed by _decorator_ and transmitted out to _sink._

On the node level, Flume is modeled around these concepts

1. Agents, are nodes that received data from the application (such as web server).
2. Processors (optional), are nodes that performed intermediate processing of the data.
3. Collectors, are nodes, that write to permanent data storage (such as HDFS, HBase, Cassandra etc).

It has these following goals that will solve aforementioned challenges in previous paragraph

1. Reliability, by providing three types of failure recovery mechanism: Best-Effort, Store on Failure and Retry, and End-to-End Reliability
2. Scalability, by applying Horizontally Scalable Data and Control Path, and Load Balancing technique.
3. Extensibility, by providing simpe source and sink API, and providing plugin architecture.
4. Manageability, by providing comprehensive interface to configure Flume and intuitive syntax to configure Flume node.

And as usual, here is the slide that prepared by our group: http://www.slideshare.net/arinto/apache-flume
