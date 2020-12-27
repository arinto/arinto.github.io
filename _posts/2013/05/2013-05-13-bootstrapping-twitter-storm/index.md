---
title: "Bootstrapping Twitter Storm"
date: "2013-05-13"
categories: 
  - "computing"
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "distributed-computing"
  - "storm"
  - "storm-concept"
  - "storm-parallelism"
  - "thesis"
  - "twitter"
  - "twitter-storm"
  - "twitter-storm-hello-world"
---

I have chances to use Twitter Storm for my thesis and in this post I would like  to give some pointers about it. I hope this will be useful for those who are starting to use Storm in their project :)

\[caption id="attachment\_901" align="aligncenter" width="245"\][![Of course I am NOT talking about this movie :D](images/perfectstormdvdcover.jpg)](http://www.otnira.com/wp-content/uploads/2013/05/perfectstormdvdcover.jpg) Of course I am NOT talking about this movie :D\[/caption\]

Well, I tried to search for Twitter Storm logo, but I could not find it. Then suddenly I remembered about the movie pictured above. Okay, let's get back to business.

### **What is Twitter Storm?**

Twitter Storm is a distributed streaming computation framework. It does, for real-time-processing(via streaming), what Hadoop's MapReduce (MR) does for batch processing. The main reason why it exists is in **inflexibility** of Hadoop MR in handling stream processing, i.e. it's too complex and error-prone to configure Hadoop MR in handling streaming data (for more detail, watch the first five minutes of this [video](http://vimeo.com/40972420 "Storm Video ETE")).

### **Fundamental Concept**

I do no want to re-invent the wheel, so please explore directly to [Concepts](http://storm.incubator.apache.org/documentation/Concepts.html "Storm Concept") section in Storm [documentation](http://storm.incubator.apache.org/documentation/Documentation.html).  I recommend you to go to Parallelism section below in order to gain more understanding of  tasks and workers. One particular aspect that I want to highlight is: Storm using **pull model**, i.e. Spout's [nextTuple](https://github.com/apache/incubator-storm/blob/v0.9.2-incubating/storm-core/src/jvm/backtype/storm/spout/ISpout.java#L90 "Spout's next tuple") method is invoked by Storm engine to pull the data to a destination bolt. And it is the same case with Bolt's [execute](https://github.com/apache/incubator-storm/blob/v0.9.2-incubating/storm-core/src/jvm/backtype/storm/task/IBolt.java#L74 "Bolt's execute") method.

### **Sample Code**

For those who prefer to see some codes in using Storm, go to [storm-starter project](https://github.com/nathanmarz/storm-starter "storm-starter project") and clone them to your local machine. Some important storm-starter classes to speed-up your learning are

1. [ExclamationTopology](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/ExclamationTopology.java "Exclamation Topology"). This is the most basic example of Storm. The topology consists of 1 spout and 2 bolts: TestWordSpout -> ExclamationBolt -> ExclamationBolt. And both connections are using shuffle-grouping. Refer to line [48 to line 52](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/ExclamationTopology.java#L48 "excalamation-topo-48") for the details in creating and connecting the components. Take a look also for the implementation of [ExclamationBolt](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/ExclamationTopology.java#L23 "ExclamationBolt").
2. [RandomSentenceSpout](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/spout/RandomSentenceSpout.java "RandomSentenceSpout"). An example of spout implementation, note that the data source is internal to the Spout, i.e. the data source is just an array of sentences initialized inside the spout.

Once you're familiar and comfortable with basic spout, bolt and topology implementation, I suggest you to start creating your own Storm components and execute your topology. A small note for storm-starter project: if you want to use Maven, rename M2-pom.xml to pom.xml and import the Maven project to you favourite editor.

Another alternative is to check other toy projects/applications in Storm such as [storm-word-count](https://github.com/arinto/storm-word-count "storm-word-count"). This example contains [spout](https://github.com/arinto/storm-word-count/blob/master/src/main/java/com/yahoo/swc/spout/TwitterSampleSpout.java "Twitter sample spout") that uses [Twitter4J](http://twitter4j.org/en/ "Twitter4J") to consume Twitter stream. [Storm's tick feature](http://www.michael-noll.com/blog/2013/01/18/implementing-real-time-trending-topics-in-storm/#excursus-tick-tuples-in-storm-08 "Storm tick") (available in 0.8+) is also used in [one of the bolts](https://github.com/arinto/storm-word-count/blob/master/src/main/java/com/yahoo/swc/bolt/ForwardDecayCountWord.java#L55 "Use of storm tick") to provide periodical action/behavior.

### **Parallelism**

Ones does not simply say that Storm parallelism is only around configuring the number of bolt or spout in the topology. There are much more. In order to be able to execute your Storm topology effectively, you need to understand the concept of _parralelism hint, executor, worker, task._ You need to be able to answer these following questions: What are they? Which one is the process? which one is the thread? what components that spouts and bolts correspond that?

But no worry, Michael Noll discussed about Storm parallelism concept in [his awesome post](http://www.michael-noll.com/blog/2012/10/16/understanding-the-parallelism-of-a-storm-topology/ "Parallelism in Storm"), which eventually transferred into Storm [documentation](http://storm.incubator.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html). Check them out!

### **Storm Development Environment**

Now we are going to discuss about development environment. In general, Storm has two modes: local and cluster. In local mode, you need to [use LocalCluster class](https://github.com/arinto/storm-word-count/blob/master/src/main/java/com/yahoo/swc/TwitterWordCountTopology.java#L136 "LocalCluster sample") and you don't need to setup anything in your development machine. The topology will be executed in a Java process in your machine. This mode is useful to perform initial testing of your topology.

In cluster mode, here are the steps (assuming you already have a Storm cluster running):

1. Download a storm release, unpack it, and put the bin folder on your path directory.
2. Put cluster information in ~/.storm/storm.yaml. At the minimum you need to setup "nimbus.host" configuration of the .yaml file.
3. Make sure you use StormSubmitter class in your code, as shown in [this example](https://github.com/arinto/storm-word-count/blob/master/src/main/java/com/yahoo/swc/TwitterWordCountTopology.java#L131 "StormSubmitter").

Wait, in step 2, I mentioned about "nimbus". Well, what is it actually? Quoting from Storm wiki [page](https://github.com/nathanmarz/storm/wiki/Setting-up-development-environment "setup development environment") (since I don't want to re-invent the wheel and keep the brevity of this post ;))

> A Storm cluster is managed by a master node called "Nimbus". Your machine communicates with Nimbus to submit code (packaged as a jar) and topologies for execution on the cluster, and Nimbus will take care of distributing that code around the cluster and assigning workers to run your topology. Your machine uses a command line client called "storm" to communicate with Nimbus. The "storm" client is only used for remote mode; it is not used for developing and testing topologies in local mode.

At this stage, it's good to know also about the topology lifecycle as explained in this [Storm wiki page](https://github.com/nathanmarz/storm/wiki/Lifecycle-of-a-topology "lifecycle of a topology").

### **Setting up Storm cluster**

Use [Michael Noll's](http://www.michael-noll.com/blog/2014/03/17/wirbelsturm-one-click-deploy-storm-kafka-clusters-with-vagrant-puppet/) [Wirbelstrum](https://github.com/miguno/wirbelsturm), or follow his tutorial [here](http://www.michael-noll.com/tutorials/running-multi-node-storm-cluster/).

Without further ado, follow this [page](https://github.com/nathanmarz/storm/wiki/Setting-up-a-Storm-cluster "setup cluster wiki") from Storm wiki. During installation, I found that these following steps are time consuming because the instructions in the wiki does not cover a case where you do not have administration access to your cluster.

1. Setup [ZeroMQ](http://www.zeromq.org/ "zeroMQ"). Remember to use ZeroMQ version 2.1.7 which you download from [here](http://download.zeromq.org/zeromq-2.1.7.tar.gz "ZeroMQ-2.1.7"). One important tips here is to call "./configure" with prefix options ("./configure --prefix="$PREFIX\_DIR", set $PREFIX\_DIR with the desired installation path). Prefix options specify where ZeroMQ put the library files from compilation, which means you do not need to have administrator privilege to setup Storm cluster.
2. Setup JZMQ. For this step, you need to download and build JZMQ from [here](http://github.com/nathanmarz/jzmq) because Storm only tested to work with this specific version of JZMQ. Again, use "--prefix" option and "--with-zeromq" option in your ./configure script. "--prefix" works the same way as zeromq setup. "--with-zeromq" flag specifies the path where zeromq is installed in your system.

A small note on Zookeeper: Storm uses library from ZooKeeper 3.3.3, so it is recommended to use ZooKeeper 3.3.3 to minimize the surprise due to different ZooKeeper version. Refer to this [thread](https://groups.google.com/forum/#!topic/storm-user/TVVF_jqvD_A "ZooKeeper in Storm ") for more info.

### **Configuration**

Once you have a Storm cluster and your Hello-World program running, it's time to do some tweaking. Explore the wiki page about [Configuration](http://storm.incubator.apache.org/documentation/Configuration.html "configuration") and check the [default values for storm.yaml file](https://github.com/apache/incubator-storm/blob/v0.9.2-incubating/conf/defaults.yaml "storm yaml") so you know that are the available options.

### **Wrap up!**

I think that's enough for now! The deadline for thesis is approaching, therefore for the following posts, I think I'll write some part of thesis, especially SAMOA design and  the parallel classifier that implemented on top of SAMOA. Ciaoooo!!
