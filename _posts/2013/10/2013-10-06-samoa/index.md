---
title: "SAMOA - Scalable Advanced Massive Online Analysis"
date: "2013-10-06"
categories: 
  - "computing"
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "distributed-streaming-machine-learning"
  - "machine-learning"
  - "samoa"
  - "thesis"
---

In this post, I'll give a quick overview of upcoming distributed streaming machine learning framework,  Scalable Advanced Massive Online Analysis (SAMOA). As I  mentioned before, SAMOA is part of my and Antonio's [theses](http://www.ac.upc.edu/emdc-master-thesis "EMDC-theses") with Yahoo! Labs Barcelona.

### What is SAMOA?

SAMOA is a tool to perform **mining on big data streams**. It is a distributed streaming machine learning  (ML) framework, i.e. it is a **Mahout but for stream mining**. SAMOA contains a **programing abstraction** for distributed streaming ML algorithms (refer to this [post](http://www.otnira.com/2013/03/28/hoeffding-tree-for-streaming-classification/ "Hoeffding Tree for Streaming Classification") for stream ML definition) to enable development of new ML algorithms without dealing with the complexity of underlying streaming processing engines (SPE, such as Twitter Storm and S4).  SAMOA also provides **extensibility** in integrating new SPEs into the framework. These features allow SAMOA users to develop distributed streaming ML algorithms once and they can execute the algorithms in multiple SPEs, i.e. **code the algorithms once and execute them in multiple SPEs.**

### Why SAMOA?

Big Data is always evolving and one of the ways to mine big data is by using the **streaming ML paradigm**. This paradigm implies that the corresponding ML model for data mining will utilize real-time feedback and ML model updates will be faster.  The ML model will adapt to changes via concept drift to handle adversarial interactions (such as spam) with the ML model. The main usage of streaming ML paradigm is to provide immediate feedback to user based on certain actions.

Concrete example  of big data stream mining is **spam detection on Yahoo! News or Yahoo! Mail**. The spams' characteristics change over time, hence we need to retrain the ML model with new arriving data. Moreover, we also need to quickly develop new ML algorithms for big data stream mining.

Unfortunately, existing solutions are not designed for big data stream. For example:

1. [Mahout](http://mahout.apache.org/ "Apache Mahout") is suitable for batch processing of the data and it is not designed for stream machine learning
2. [Massive Online Analysis (MOA)](http://moa.cms.waikato.ac.nz/ "MOA") is suitable for stream machine learning, but it does  not scale. It is only able to execute on a single machine and it could no be scaled into multiple machines if needed.

Furthermore, there is an existing solution called [Jubatus](http://jubat.us/en/ "Jubatus"), which is a distributed machine learning framework for big data stream. However, Jubatus implementation is tightly coupled between distributed ML algorithms and the underlying distributed streaming computation platform.

Table below summarizes existing machine learning framework and their characteristics related to mining big data streams:

\[table id=1 /\]

SAMOA addresses the aforementioned limitations of existing frameworks and tools by:

1. SAMOA is a framework that executes on top of distributed streaming computation platforms, such as Storm and S4. Hence, SAMOA **inherits the scalability of the underlying platform**.
2. SAMOA utilizes **stream ML paradigm** and it contains collection of streaming ML algorithms. Hence it is suitable to perform real-time analytics.
3. SAMOA decouple the ML algorithms with the underlying distributed streaming computation platforms, which means we can easily deploy SAMOA in different platforms. In other words, SAMOA has **"write once, deploy everywhere"** paradigm, i.e. we only need to write the ML algorithms once using SAMOA API and we can easily deploy the algorithms in different types of cluster.

To conclude, SAMOA **scales horizontally, is designed for streaming ML paradigm** (SAMOA focuses on speed/real-time analytics), and **is loosely coupled** with its underlying distributed computation platform.

### How to use SAMOA?

Now we arrive into the most important question, how to use SAMOA? There are **three main use cases of SAMOA** as followings:

#### 1\. SAMOA as a collection of distributed streaming ML algorithms

SAMOA users could use the readily available algorithms in SAMOA. They could execute the algorithms inside SAMOA using the supported streaming computation platforms (currently they are Storm and S4) or in local mode. The use case is similar to WEKA and MOA where users issue command lines to execute certain algorithms under desired settings. Using SAMOA 0.0.1 (still in development and not yet released to public yet, so stay tuned and be patient ), example of the command line is:

`/samoa storm ./SAMOA-Storm-0.0.1.jar "PrequentialEvaluation -d /mnt/scratch/dump.csv -i 1000000 -f 100000 -l (trees.VerticalHoeffdingTree -p 4) -s (generators.RandomTreeGenerator -c 2 -o 10 -u 10)"`

The aforementioned command means "We use SAMOA version 0.0.1 on top of Storm to execute [Prequential Evaluation](http://en.wikipedia.org/wiki/Data_stream_mining) with Vertical Hoeffding Tree learner (it's a [Hoeffding Tree algorithm](http://www.otnira.com/2013/03/28/hoeffding-tree-for-streaming-classification/ "Hoeffding Tree for Streaming Classification") with [Vertical Parallelism](http://www.otnira.com/2013/08/03/parallelism-for-dsml/ "Parallelism for Distributed Streaming ML")) and the data source is Random Tree Generator"

The next natural question is how do we observe the results of the execution? Since SAMOA executes on top of distributed platform, we should observe the results from the platform log files. Yes, we know that it is not ideal and we will take note this matter as  something that should be improved. The ideal solution that we envision is to follow Storm UI in which there will be web UI that gather all statistics from each SAMOA distributed component that executes the algorithm.

#### 2\. Developing new ML algorithms using SAMOA

SAMOA provides abstractions to develop new distributed streaming ML algorithms. ML engineers and scientists develop the algorithm in term of **ProcessingItems (PI)**, **Processors** and **Streams** i.e. the algorithms consists of a network of PIs and Streams. In SAMOA, we call this network as a **Topology**.

Each PI wraps a Processor and a Processor contains the business logic of the algorithm i.e. the ML algorithm developer needs to provide the implementation of each processor in the topology. Let's say we have a simple word-count algorithm, the corresponding Topology in SAMOA will be something like this:

\[caption id="attachment\_1012" align="aligncenter" width="576"\][![Word Count Topology](images/word-count.png)](http://www.otnira.com/wp-content/uploads/2013/09/word-count.png) Word Count Topology\[/caption\]

Refer to above figure, entrance PI contains the data source for this topology. It could read from text file, or get the real-time tweets from Twitter. Splitter PI needs to send the each word to the correct word-counter PI. SAMOA abstracts the message sent between PIs as ContentEvents. In our example, each ContentEvent contains the data (which is the split word) and the key. To understand the key's role, we need to discuss the concept of Parallelism Hint and Grouping.

Parallelism Hint is the number of actual Processing Item runtime processes that execute in the SAMOA execution cluster. The Grouping mechanism determines how data distribution between PIs. In our previous example, we have parallelism hint equals to 4 and we use key-grouping between splitter PI and word-counter PI.

[![shuffle-grouping](images/shuffle-grouping.png)](http://www.otnira.com/wp-content/uploads/2013/09/shuffle-grouping.png) [![key-grouping](images/key-grouping.png)](http://www.otnira.com/wp-content/uploads/2013/09/key-grouping.png)

[![all-grouping](images/all-grouping2.png)](http://www.otnira.com/wp-content/uploads/2013/09/all-grouping2.png)

As shown in figure above, SAMOA supports several types of Grouping at this moment (and this subjects to change in the future releases):

1. **Shuffle** grouping, or round robin of content events.
2. **Key** grouping, that is each content event that has the same key always goes to the same PIs.
3. **All** grouping, that is broadcasting of content events to all Processing Items.

Code snippet below shows the sample code of word-count topology example in this section.

\[caption id="attachment\_1020" align="aligncenter" width="535"\][![Word-count code snippet in SAMOA](images/Pseudocode.png)](http://www.otnira.com/wp-content/uploads/2013/09/Pseudocode.png) Word-count topology  in SAMOA\[/caption\]

#### 3\. Extending SAMOA with new SPEs

We designed SAMOA to be **loose-coupling** with the underlying stream processing engines (SPEs). Currently  SAMOA supports Storm and S4. To integrate new SPEs, developer could take a look at the **SPE-adapter layer** (refer to section 5.3 of my [thesis](https://dl.dropboxusercontent.com/u/19929044/SAMOA/thesis/arinto-thesis.pdf "Arinto-thesis")) and implement all the necessary components of SAMOA in term of the underlying SPE. Here is the concrete example from Storm integration where we map SAMOA components into Storm components and tweak them accordingly.

\[table id=2 /\]

\*Internally, SAMOA for Storm has two types of Stream, which are StormSpoutStream and StormBoltStream. However, they are transparent to users who use SAMOA API to develop ML algorithms. The users only see SAMOA's Stream interface.

### Wrap up!

To wrap up this post, I list the additional resources about SAMOA. They are

1. [SAMOA website](http://samoa-project.net/ "SAMOA project"). Take a look at the [slides](https://speakerdeck.com/gdfm/samoa-a-platform-for-mining-big-data-streams "SAMOA-slides") by Gianmarco. It explains SAMOA and motivation behind it. The slides were presented in WWW'13.
2. For more details about SAMOA, you can take a look at [my thesis](http://www.slideshare.net/arinto/emdc-thesis "emdc-thesis") and Antonio's thesis (I'll update with the link once he publishes his thesis). And here is the corresponding [slides](http://www.slideshare.net/arinto/final-presentation-34) for my thesis defense.

We plan to release and open-source SAMOA soon!. I'll update this post once the code is published.  Moving forward, I think this post will be the last post regarding my thesis :).  I'll continue writing about come computing stuffs, traveling and soccer, as the blog's tag suggests :)

Ciao y hasta pronto!!
