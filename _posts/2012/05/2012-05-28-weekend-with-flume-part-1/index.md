---
title: "Weekend with Flume - Part 1"
date: "2012-05-28"
categories: 
  - "computing"
  - "school"
tags: 
  - "apache"
  - "channel"
  - "flume"
  - "hadoop"
  - "log"
  - "sink"
  - "source"
---

I was in geeky mode this weekend, spending most of my time configuring Flume for our SDS project. I'll share some observation and tricks that our group did in configuring Flume

\[caption id="" align="aligncenter" width="170"\]![Apache Flume](images/flume-logo.jpeg "Apache Flume") Apache Flume\[/caption\]

#### Flume Primer

Quoting definition from [Apache's Flume Wiki](https://cwiki.apache.org/confluence/display/FLUME/Home):

> Apache Flume is a distributed, reliable, and available service for efficiently collecting, aggregating, and moving large amounts of log data. Its main goal is to deliver data from applications to Apache Hadoop's HDFS. It has a simple and flexible architecture based on streaming data flows. It is robust and fault tolerant with tunable reliability mechanisms and many failover and recovery mechanisms. The system is centrally managed and allows for intelligent dynamic management. It uses a simple extensible data model that allows for online analytic applications.

When you're looking for some resources for Flume, most of the time you will find two type of resources

1. **Flume 0.9.x**. This version of Flume is sometimes referred as Flume OG (old generation, maybe :p). I have some introductory slides of this Flume in [this post](http://www.otnira.com/2012/03/20/apache-flume/).
2. **Flume 1.x.** It is referred as Flume NG (new generation). We are using this version in our project. Therefore the remaining content of this post will refer to Flume NG.

We found some comprehensive references in configuring Flume NG. They are

1. Flume NG - [Getting Started](https://cwiki.apache.org/confluence/display/FLUME/Getting+Started).
2. User Guide, can be downloaded from [here](https://cwiki.apache.org/confluence/display/FLUME/Flume+1.x+Documentation).

#### Flume Installation

In our project, we use Flume 1.x shipped with Cloudera Distribution with Hadoop (CDH) version 3. Installation instruction can be found [here](https://ccp.cloudera.com/display/CDHDOC/Flume+1.x+Installation). It's pretty comprehensive and we followed all the steps there.

Other alternative is by downloading the source and build it using Maven from [here](https://www.apache.org/dyn/closer.cgi/incubator/flume/flume-1.1.0-incubating/).

We are  using [Cloudera Manager](https://ccp.cloudera.com/display/SUPPORT/Downloads) (Free Edition of course :p) to setup our Flume and Hadoop cluster in Amazon Web Service. But, in the middle of the project, we found that \*maybe\* we can simplify cluster setup by using [Amazon Virtual Private Cloud](http://aws.amazon.com/vpc/) (VPC) and [Elastic Map Reduce](http://aws.amazon.com/elasticmapreduce/).

Important files and folders after installation using CDH 3:

1. `/etc/flume-ng/conf/flume.conf` -> contains the configuration of our Flume agent. We can configuration our source, sink, and channel. This is the file that we will always change! Default configuration file can be copied from `/etc/flume-ng/conf/flume-conf.properties.template`
2. `/var/log/flume-ng/` -> contains Flume log files. It is very useful to clear this folder from log file before you run your Flume agent, so you can easily see the log of your Flume agent execution using `tail` command.
3. `/etc/init.d/flume-ng-agent` -> shell script to execute Flume agent easily. But be careful! The name of your Flume agent should be `agent` in order to use this default script. This name can be set in the `flume.conf` file. If you change the name in `flume.conf` you need to tweak this script.

#### Executing Flume for First Time

Use default configuration file (without modifying it), start Flume agent using the `flume-ng-agent` script. Default configuration file will setup your source as Sequence Generator source, and the sink to Logger. You can observe the output of Sequence Generator in `flume.log`. Observe that there should be no `exception` detected in your log files.

Refer to this snapshot below for Flume default configuration file

<script type="text/javascript" src="http://pastebin.com/embed_js.php?i=hbbKsuE2"></script>

Start running Flume using this command

<script type="text/javascript" src="http://pastebin.com/embed_js.php?i=4i8nBQyq"></script>

You should see the output in `flume.log` as shown in [this link](http://pastebin.com/AqC8hYWa).

#### Configuring Executable as Flume Agent Source

After successfully running Flume using default configuration, our next step is trying to set our own executable as Flume Agent Source. We created this following simple C code to print `line #runningNumber` every some interval. The C code is shown below. Note that `\r` is used because we don't want to make `stdout` out of memory and the executable is able to execute in super long time.

<script type="text/javascript" src="http://pastebin.com/embed_js.php?i=MdhaqCxf"></script>

We modified the Flume configuration file (`flume.conf`) as shown below. Note that the name of our agent is `exec-agent` which means if you need to modify `flume-ng-agent` script to use `exec-agent`as Flume agent name.

<script type="text/javascript" src="http://pastebin.com/embed_js.php?i=9uEEi1Mg"></script>

Lesson learned in configuring execution source is you need to provide **absolute path to the executable** so that you get rid of `PATH` setting and issues.

The resulting output in `flume.log` can be found in this [following link](http://pastebin.com/RjYNnDS3). Note that in the logger sink, it displays `line #runningNumber` as printed by the C code.

I think it is enough for today.. Hehe.. Next post I plan to covered how to configure Flume Agent's source and sink using Avro plugin in [this post](http://www.otnira.com/2012/05/30/weekend-with-flume-part-2/ "Weekend with Flume – Part 2").
