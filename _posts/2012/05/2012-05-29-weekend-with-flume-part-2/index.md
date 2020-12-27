---
title: "Weekend with Flume - Part 2"
date: "2012-05-29"
categories: 
  - "computing"
  - "school"
tags: 
  - "apache-flume"
  - "avro-sink-configuration"
  - "avro-source-configuration"
  - "flume"
  - "flume-based-news-aggregator"
  - "hdfs"
  - "hdfs-sink-configuration"
---

After covering some of basic configurations of Flume in [Part 1 of Weekend with Flume series](http://www.otnira.com/2012/05/28/weekend-with-flume-part-1/), I'll cover Avro Source, Avro Sink and HDFS sink in this post. Let's pick a scenario from our school project below. We setup this configuration in Amazon Web Service (AWS), but I will not discuss about our experiences with AWS in this post.

\[caption id="attachment\_335" align="aligncenter" width="579" caption="Flume-based News Aggregator"\][![Flume-based News Agregator](images/SystemArchitecture.png "Flume-based News Agregator")](http://www.otnira.com/wp-content/uploads/2012/05/SystemArchitecture.png)\[/caption\]

#### Avro Sink Configuration

We have **Agent#1** connecting to **Collector** through a pair of Avro Sink and Avro Source. To achieve this configuration, we have this following flume configuration file in **Agent#1**. Note that for Avro Sink to work, you need to specify the IP address of destination agent and which port its Avro Source listens. Our RSS Feeder is writing into a temporary file, that's why Agent#1's source configuration is using execution source.

<script type="text/javascript" src="http://pastebin.com/embed_js.php?i=XWn2VAZf"></script>

#### Avro Source and HDFS Sink Configuration

Next is **Collector's** configuration: Avro Source and HDFS sink as shown in configuration file below. Collector's Avro Source needs to have the same port as defined by the Agent#1's configuration file. And `bind` parameter is set to Collector's IP address. Setting up HDFS sink is a bit tricky. You should ensure your HDFS is up and running, and it has the folder that we specify in our configuration file. You also should ensure that the folder has its owner set to `flume` since HDFS sink will access HDFS using `flume` user name. HDFS sink has several options that we left them into default value. You can consult Flume User Guide from [this link](https://cwiki.apache.org/confluence/display/FLUME/Flume+1.x+Documentation). Here is the configuration file for Collector.

<script type="text/javascript" src="http://pastebin.com/embed_js.php?i=wHzVdraC"></script>

 If you configure HDFS sink correctly, you should see no error in Collector Flume log file `/var/log/flume-ng/flume.log` and you should see the HDFS transaction that the sink perform. Then you also can check HDFS (through NameNode and DataNode web UI) whether the data can be stored properly as shown in snapshot below. Note that the file name is "FlumeData.xxxxxxxxx", stored in **flume** folder of HDFS. File name is configurable via Flume configuration file.

\[caption id="attachment\_342" align="aligncenter" width="555" caption="HDFS Content"\][![HDFS Content](images/HDFS-Content.png "HDFS Content")](http://www.otnira.com/wp-content/uploads/2012/05/HDFS-Content.png)\[/caption\]

We didn't have enough time to play around with channel's capacity and transaction capacity but we suspect that collector should have cumulative capacity value of agents' values that connected to it. Another thing that is interesting to explore is channel type. Other than Memory channel type, Flume also has JDBC type as well as FileChannel type that have more reliability compared to Memory channel. Well, more investigations are needed on these cases. But unfortunately we don't have that much of time.

To conclude, setting up Flume is fairly easy, although we still need more reading to make better configuration of it. Hope these two posts are useful for humanity and I plan to cover our Flume-based News Aggregator project in upcoming posts. Stay tuned! :)

_ps: Flume-based News Aggregator project is done by me, [Mário](http://www.aknahs.pt/) and [Zafar](http://115.186.131.91/~zafar/about.html) as one of our school projects._
